# Questions
1. Could you please introduce yourself?
2. Could you please give me a recent experience of yours?
3. How would you choose between **ECS and EKS** for a new production workload?
4. How would you design a secure and highly available ECS form?
5. How would you structure the Terraform for a large organization?
6. How do you manage the Terraform State securely?
7. How do you prevent the Terraform drift and unsafe changes?
8. How would you design a secure GitHub Actions deployment pipeline for AWS?
9. What is the difference between reusable workflows and composite actions?
10. How would you debug Actions pipeline that works locally but fails in CI?
11. What Git branching strategy would you recommend?
12. How would you recover from an incorrect production merge?
13. Could you explain what merge, rebase, and cherry pick?
14. How would you write safe automation script for deleting unused AWS resources?
15. How do you write reliable, safe scripts for CI/CD?
16. How do you monitor an ECS-based application using Dynatrace?
17. How do you investigate a certain latency increase using Dynatrace?
18. How do you prevent monitoring alerts from becoming noisy?



Could you please introduce yourself?

Answer: The candidate introduced themselves with 16 years of experience in AWS cloud engineering, emphasizing expertise in containerization, CI/CD pipelines (GitHub Actions, ArgoCD), Terraform, Ansible, Docker, and Kubernetes.

Could you please give me a recent experience of yours?

Answer: The candidate described implementing secure Terraform state management (S3 backend with versioning and IAM policies) and modernizing CI/CD using GitHub Actions and ArgoCD for zero-touch deployments.

How would you choose between X and X for a new production workload?

Answer: The candidate recommended ECS for simplicity, operational overhead reduction, and easy AWS integration, especially for 20 stateless APIs with fewer than 100 daily deployments.

How would you design a secure and highly available ECS form?

Answer: The candidate suggested using multi-AZ deployments, IAM least privilege, KMS for encryption, private subnets, NAT gateways, and VPC endpoints for S3 access.

How would you structure the Terraform for a large organization?

Answer: The candidate advocated modularizing Terraform code, using separate state files per environment, remote state management via S3 with versioning, and drift detection through regular plan executions.

How do you manage the Terraform State securely?

Answer: The candidate explained using DynamoDB for state locking, encryption, IAM policies, versioning, and regular backups.

How do you prevent the Terraform drift and unsafe changes?

Answer: The candidate described a script to detect drift, authorize changes, and reapply unauthorized modifications.

How would you design a secure GitHub Actions deployment pipeline for AWS?

Answer: The candidate outlined using OIDC for authentication, least privilege access, Vault for secrets, branch protection, PR reviews, unit testing, static code analysis, image scanning, and ArgoCD integration for production deployments.

What is the difference between reusable workflows and composite actions?

Answer: The candidate defined reusable workflows as centralized pipelines callable by multiple projects and composite actions as custom actions grouping multiple steps (e.g., shell commands).

How would you debug Actions pipeline that works locally but fails in CI?

Answer: The candidate recommended checking logs for errors, validating dependencies, verifying secrets/permissions, and replicating issues locally.

What Git branching strategy would you recommend?

Answer: The candidate suggested a trunk-based strategy with main for stable deployments, release for enhancements, short-lived feature branches, and hotfixes for emergencies.

How would you recover from an incorrect production merge?

Answer: The candidate proposed identifying the faulty commit, comparing files, and manually committing fixes.

Could you explain what merge, rebase, and cherry pick?

Answer: The candidate defined merge as integrating branches with history retention, rebase as linearizing history onto another branch, and cherry pick as applying specific commits.

How would you write safe automation script for deleting unused AWS resources?

Answer: The candidate described using Python/Boto3 to filter resources, generate dry-run reports (e.g., CSV/email), and act on approved changes.

How do you write reliable, safe scripts for CI/CD?

Answer: The candidate emphasized handling edge cases (e.g., undefined variables, command failures), implementing retries for transient issues (network timeouts, rate limiting), and using status codes for error handling.

How do you monitor an ECS-based application using Dynatrace?

Answer: The candidate explained enabling Dynatrace auto-discovery, configuring service detection/tagging, setting up dashboards/alerts, and using log aggregation with Prometheus/Grafana for cluster monitoring.

How do you investigate a certain latency increase using Dynatrace?

Answer: The candidate outlined monitoring distribution tracing, analyzing service flows, checking resource metrics (CPU, memory, disk), and collaborating with application teams to resolve backend issues.

How do you prevent monitoring alerts from becoming noisy?

Answer: The candidate recommended setting meaningful thresholds, implementing alert suppression, grouping related events, and using incident management to reduce unnecessary alerts.