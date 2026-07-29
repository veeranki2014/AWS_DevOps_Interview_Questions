## how I typically create a GitHub Actions workflow using AWS services for application and infrastructure deployments.
1. **Define the workflow as code**
   - I create a YAML workflow such as **.github/workflows/aws-deployment.yml**
   - Keeping the workflow with the source code provides version control, traceability, and peer review.
   
2. **Configure workflow triggers**
   - I use the on section to execute the workflow for events such as:
   - **Pull requests** to validate changes
   - Commits to main or develop
   - Manual deployments using **workflow_dispatch**
   - Scheduled executions
   
3. **Select the runner**
    - I typically use GitHub-hosted runners such as ubuntu-latest.
    - I use self-hosted runners when the pipeline requires private VPC connectivity, specialized software, additional performance, or stricter organizational controls.
   
4. **Authenticate securely with AWS**
    - I configure GitHub as an OIDC identity provider in AWS IAM and create an IAM role that the workflow can assume.
    - This allows GitHub Actions to request temporary AWS credentials instead of storing permanent AWS access keys in repository secrets. The role receives only the permissions required for the deployment. GitHub’s OIDC documentation and AWS IAM documentation describe this authentication model.
   
5. **Define build, test, and deployment jobs**
    - A typical workflow contains:
    - Build: Compile the application and install dependencies.
    - Test: Run unit, integration, quality, and security tests.
    - Package: Build a ZIP artifact or Docker image.
    - Infrastructure: Validate, plan, and apply Terraform or deploy CloudFormation/CDK.
    - Deployment: Deploy the application to the appropriate AWS service.
    - Validation: Run smoke tests and verify application health.
    - Jobs can run in parallel or sequentially using needs.
   
6. **Integrate AWS services**

    Depending on the application, I integrate services such as:
    Amazon ECR: Store Docker images
    Amazon ECS or EKS: Run containerized applications
    AWS Lambda: Run serverless applications
    Amazon S3 and CloudFront: Host and distribute static applications
    Elastic Beanstalk: Deploy managed web applications
    AWS CodeDeploy: Perform rolling or blue/green deployments
    AWS Secrets Manager or Systems Manager Parameter Store: Manage runtime secrets
    Amazon CloudWatch: Collect logs, metrics, and alarms
   7. **Provision infrastructure as code**
   For Terraform, I add steps for:
   terraform fmt -check
   terraform init
   terraform validate
   terraform plan
   terraform apply
   I commonly store Terraform state in Amazon S3 and use state locking where supported by the selected backend configuration. Pull requests create a plan, while apply is limited to protected branches and approved environments.
   8. **Implement environments and approvals**
   I define GitHub environments such as development, qa, and production.
   Production environments can require reviewers and enforce deployment branch restrictions.
   Each environment can use a different AWS IAM role, account, or region.
   In larger organizations, I normally deploy Dev, QA, and Production into separate AWS accounts.
   9. **Monitor and handle rollback**
   After deployment, I:
   Run application health checks and smoke tests.
   Review CloudWatch logs, alarms, and deployment events.
   Send notifications through Amazon SNS, email, Slack, or Microsoft Teams.
   Roll back to the previous ECS task definition, Lambda version, artifact, or infrastructure release if validation fails.
   Example: Build and deploy a Docker application to Amazon ECR and ECS
   name: AWS CI/CD Pipeline

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  id-token: write

env:
  AWS_REGION: us-east-1
  ECR_REPOSITORY: my-application
  ECS_CLUSTER: production-cluster
  ECS_SERVICE: application-service

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Run tests
        run: |
          echo "Installing dependencies..."
          echo "Running unit and security tests..."

  build-and-push:
    if: github.event_name == 'push'
    needs: build-and-test
    runs-on: ubuntu-latest

    outputs:
      image: ${{ steps.build-image.outputs.image }}

    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Configure temporary AWS credentials
        uses: aws-actions/configure-aws-credentials@v6
        with:
          role-to-assume: ${{ vars.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Sign in to Amazon ECR
        id: ecr-login
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Docker image
        id: build-image
        env:
          ECR_REGISTRY: ${{ steps.ecr-login.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          IMAGE="$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"
          docker build -t "$IMAGE" .
          docker push "$IMAGE"
          echo "image=$IMAGE" >> "$GITHUB_OUTPUT"

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Configure temporary AWS credentials
        uses: aws-actions/configure-aws-credentials@v6
        with:
          role-to-assume: ${{ vars.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Deploy new image to ECS
        run: |
          aws ecs update-service \
            --cluster "$ECS_CLUSTER" \
            --service "$ECS_SERVICE" \
            --force-new-deployment

          aws ecs wait services-stable \
            --cluster "$ECS_CLUSTER" \
            --services "$ECS_SERVICE"
In a production implementation, I update the ECS task definition with the newly built image URI before updating the ECS service. This ensures the service deploys the exact image identified by the Git commit SHA.
**Summary of my experience**
   - I create GitHub Actions workflows for complete CI/CD automation on AWS.
   -  I integrate services such as IAM, ECR, ECS, EKS, Lambda, S3, CloudFront, Secrets Manager, and CloudWatch.
   - I provision infrastructure using Terraform, CloudFormation, or AWS CDK.
   - I use OIDC, temporary credentials, least-privilege IAM roles, protected environments, approvals, and separate AWS accounts to secure deployments.
    - I support multi-environment releases, automated testing, monitoring, blue/green deployments, and rollback strategies.
---
## how GitHub Actions securely connects to AWS using OpenID Connect, or OIDC. The main advantage is that we don’t need to store long-lived AWS access keys in GitHub.


The GitHub Actions workflow starts
A workflow is triggered by an event such as a pull request, a push to main, or a manual deployment.

**GitHub generates an OIDC token**

    The workflow requests a signed JSON Web Token from GitHub’s OIDC provider. For this, the workflow requires:
    permissions:
      contents: read
      id-token: write
    The token contains claims identifying the repository, branch, organization, workflow, or GitHub environment initiating the request.

**The workflow requests an AWS role**

    GitHub Actions sends the token to AWS Security Token Service and calls AssumeRoleWithWebIdentity.
    The workflow typically uses:
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v6
      with:
        role-to-assume: arn:aws:iam::123456789012:role/GitHubActionsRole
        aws-region: us-east-1

**AWS validates the request**

    AWS STS verifies that:
    GitHub issued and signed the token.
    The token audience is sts.amazonaws.com.
    The repository, branch, or environment matches the IAM role’s trust policy.
    The workflow is authorized to assume that specific role.

**AWS returns temporary credentials**

    If validation succeeds, STS provides a temporary:
    Access key ID
    Secret access key
    Session token
    These credentials expire automatically after a limited period.

**The workflow accesses AWS services**

    GitHub Actions uses the temporary credentials to perform only the operations allowed by the IAM role’s permission policy—for example:
    Push an image to Amazon ECR
    Deploy a service to Amazon ECS or EKS
    Upload files to Amazon S3
    Update a Lambda function
    Run Terraform against AWS

**Trust policy vs. permission policy**

    This distinction is important in interviews:
    The trust policy answers: “Who can assume this IAM role?”
    The permission policy answers: “What can that role do after it is assumed?”
    For example, the trust policy might allow only the main branch of one GitHub repository:
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
          "token.actions.githubusercontent.com:sub":
            "repo:my-organization/my-repository:ref:refs/heads/main"
        }
      }
    }

#### Why I prefer OIDC
    “I prefer OIDC because it eliminates permanent AWS credentials from GitHub secrets. AWS provides short-lived credentials for each workflow run, and access is controlled through IAM trust conditions and least-privilege permission policies. This reduces the risk of credential leakage and makes rotation unnecessary.”

**Important security practices**

    In production, I would:
    Restrict the trust policy to specific repositories, branches, or GitHub environments.
    Avoid wildcard conditions such as allowing every repository.
    Use separate IAM roles for development, QA, and production.
    Use separate AWS accounts where possible.
    Give each role only the required permissions.
    Protect the GitHub production environment with required reviewers.
    Avoid printing tokens or credentials in workflow logs.
    Use CloudTrail to audit role assumptions and AWS API activity.
**Short interview answer**

    “I integrate GitHub Actions with AWS using OIDC. The workflow requests a signed OIDC token from GitHub and presents it to AWS STS using AssumeRoleWithWebIdentity. STS validates the token against the IAM role’s trust policy, including the repository, branch, and audience claims. If validation succeeds, AWS returns short-lived credentials. The workflow then uses those credentials to access services such as ECR, ECS, S3, or Lambda according to the role’s permission policy. This is more secure than storing permanent AWS access keys because the credentials are temporary and access can be tightly restricted.”