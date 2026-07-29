## how I typically create a GitHub Actions workflow using AWS services for application and infrastructure deployments.
1. **Define the workflow as code**
I create a YAML workflow such as **.github/workflows/aws-deployment.yml**
Keeping the workflow with the source code provides version control, traceability, and peer review.
2. **Configure workflow triggers**
I use the on section to execute the workflow for events such as:
Pull requests to validate changes
Commits to main or develop
Manual deployments using workflow_dispatch
Scheduled executions
3. Select the runner
I typically use GitHub-hosted runners such as ubuntu-latest.
I use self-hosted runners when the pipeline requires private VPC connectivity, specialized software, additional performance, or stricter organizational controls.
4. Authenticate securely with AWS
I configure GitHub as an OIDC identity provider in AWS IAM and create an IAM role that the workflow can assume.
This allows GitHub Actions to request temporary AWS credentials instead of storing permanent AWS access keys in repository secrets. The role receives only the permissions required for the deployment. GitHub’s OIDC documentation and AWS IAM documentation describe this authentication model.
5. Define build, test, and deployment jobs
A typical workflow contains:
Build: Compile the application and install dependencies.
Test: Run unit, integration, quality, and security tests.
Package: Build a ZIP artifact or Docker image.
Infrastructure: Validate, plan, and apply Terraform or deploy CloudFormation/CDK.
Deployment: Deploy the application to the appropriate AWS service.
Validation: Run smoke tests and verify application health.
Jobs can run in parallel or sequentially using needs.
6. Integrate AWS services
Depending on the application, I integrate services such as:
Amazon ECR: Store Docker images
Amazon ECS or EKS: Run containerized applications
AWS Lambda: Run serverless applications
Amazon S3 and CloudFront: Host and distribute static applications
Elastic Beanstalk: Deploy managed web applications
AWS CodeDeploy: Perform rolling or blue/green deployments
AWS Secrets Manager or Systems Manager Parameter Store: Manage runtime secrets
Amazon CloudWatch: Collect logs, metrics, and alarms
7. Provision infrastructure as code
For Terraform, I add steps for:
terraform fmt -check
terraform init
terraform validate
terraform plan
terraform apply
I commonly store Terraform state in Amazon S3 and use state locking where supported by the selected backend configuration. Pull requests create a plan, while apply is limited to protected branches and approved environments.
8. Implement environments and approvals
I define GitHub environments such as development, qa, and production.
Production environments can require reviewers and enforce deployment branch restrictions.
Each environment can use a different AWS IAM role, account, or region.
In larger organizations, I normally deploy Dev, QA, and Production into separate AWS accounts.
9. Monitor and handle rollback
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
Summary of my experience
I create GitHub Actions workflows for complete CI/CD automation on AWS.
I integrate services such as IAM, ECR, ECS, EKS, Lambda, S3, CloudFront, Secrets Manager, and CloudWatch.
I provision infrastructure using Terraform, CloudFormation, or AWS CDK.
I use OIDC, temporary credentials, least-privilege IAM roles, protected environments, approvals, and separate AWS accounts to secure deployments.
I support multi-environment releases, automated testing, monitoring, blue/green deployments, and rollback strategies.
