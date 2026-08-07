## AWS
1. Explain the AWS shared responsibility model.
2. What is the difference between IAM users, groups, roles, and policies?
3. How would you securely grant an EC2 instance access to S3?
4. Explain public and private subnets, route tables, IGWs, and NAT gateways.
5. What is the difference between security groups and network ACLs?
6. How do VPC peering, Transit Gateway, and PrivateLink differ?
7. Compare Application, Network, and Gateway Load Balancers.
8. How does EC2 Auto Scaling work, and which scaling policies have you used?
9. Compare EBS, EFS, and S3 and describe suitable use cases.
10. Explain S3 versioning, lifecycle policies, encryption, and bucket policies.
11. How do RDS Multi-AZ and read replicas differ?
12. How would you design a highly available application across Availability Zones?
13. What happens when an EC2-hosted application becomes unreachable?
14. How do Route 53 routing policies and health checks work?
15. Compare CloudWatch, CloudTrail, AWS Config, and VPC Flow Logs.
16. How do you manage secrets using Secrets Manager or Parameter Store?
17. How do KMS keys and envelope encryption work?
18. How would you reduce AWS costs without affecting availability?
19. Explain ECS, EKS, and Lambda and when you would use each.
20. Describe an AWS disaster-recovery strategy using RTO and RPO.

## Terraform

1. What is Infrastructure as Code, and why use Terraform?
2. Explain providers, resources, data sources, variables, and outputs.
3. What happens during `terraform init`, `plan`, and `apply`?
4. What information does the Terraform state file contain?
5. Why should Terraform state be stored remotely?
6. How do S3 state storage and DynamoDB locking work?
7. How do you protect sensitive information in Terraform state?
8. What is state drift, and how do you detect and correct it?
9. How do you import existing infrastructure into Terraform?
10. Explain implicit and explicit resource dependencies.
11. What are Terraform modules, and how should they be structured?
12. Compare `count` and `for_each`.
13. What are workspaces, and when should they be avoided?
14. How do `locals`, dynamic blocks, and conditional expressions work?
15. What does the `lifecycle` block do?
16. What happens if a Terraform apply fails midway?
17. How would you recover from a lost or corrupted state file?
18. How do you manage separate development, staging, and production environments?
19. How do you validate, test, scan, and format Terraform in CI?
20. How do you upgrade providers or modules without causing downtime?

## Linux

1. Describe the Linux boot process.
2. What is the difference between a process and a thread?
3. How do you identify which process is consuming excessive CPU or memory?
4. Explain load average and how it differs from CPU utilization.
5. How do you find which process is listening on a particular port?
6. Explain file permissions, ownership, `chmod`, `chown`, and `umask`.
7. What are SUID, SGID, and the sticky bit?
8. Explain hard links and symbolic links.
9. How do you identify and resolve a full filesystem?
10. What are inodes, and how can a filesystem run out of them?
11. How do you find files modified recently or larger than a given size?
12. How do you troubleshoot a service that fails to start?
13. Explain systemd units, targets, dependencies, and journal logs.
14. How do `grep`, `awk`, `sed`, `cut`, `sort`, and `xargs` differ?
15. What happens when you execute a command in the shell?
16. How do DNS resolution and `/etc/resolv.conf` work?
17. Explain Linux signals, zombie processes, and orphan processes.
18. What are file descriptors, and how do you investigate exhausted descriptors?
19. How do you troubleshoot network connectivity between two Linux servers?
20. Write a shell script that checks a service and sends an alert or restarts it.

## GitHub Actions

1. What are workflows, events, jobs, steps, and actions?
2. What is the difference between GitHub-hosted and self-hosted runners?
3. How do jobs run sequentially or in parallel?
4. How do `needs`, conditions, and job outputs work?
5. How do you securely manage secrets in GitHub Actions?
6. What are environments, protection rules, and environment secrets?
7. What is a matrix strategy, and when would you use it?
8. How do reusable workflows differ from composite actions?
9. How do caching and workflow artifacts differ?
10. How do you trigger workflows manually or from another workflow?
11. How do branch, path, and tag filters work?
12. How would you build and push a Docker image from a workflow?
13. How would you deploy to AWS without storing long-lived credentials?
14. What permissions does `GITHUB_TOKEN` have, and how do you restrict them?
15. How do you prevent concurrent deployments to the same environment?
16. How do you pass data between steps and jobs?
17. How do you handle workflow failures, retries, and cleanup steps?
18. How do you protect against untrusted pull-request workflow code?
19. How would you design CI/CD with testing, approval, deployment, and rollback?
20. How do you debug a workflow that works locally but fails on a runner?

## Docker

1. What problems do containers solve?
2. How do containers differ from virtual machines?
3. Explain Docker images, containers, layers, and registries.
4. What happens internally when you run `docker run`?
5. What is the difference between `CMD` and `ENTRYPOINT`?
6. What is the difference between `COPY` and `ADD`?
7. How do you write an efficient and secure Dockerfile?
8. What are multi-stage builds, and why are they useful?
9. How does Docker layer caching work?
10. Explain bridge, host, none, and overlay networking.
11. What are Docker volumes and bind mounts?
12. How do you persist data generated by a container?
13. How do you pass configuration and secrets to containers securely?
14. Why does a container exit immediately after starting?
15. How do you troubleshoot an unhealthy or unreachable container?
16. How do Linux namespaces and cgroups support containers?
17. Why should containers normally run as a non-root user?
18. How do you reduce image size and vulnerability exposure?
19. What are health checks, restart policies, and resource limits?
20. How does Docker Compose differ from Kubernetes?

## Kubernetes

1. Explain the Kubernetes control-plane and worker-node components.
2. What happens internally after running `kubectl apply`?
3. Compare Pods, Deployments, StatefulSets, DaemonSets, and Jobs.
4. Compare ClusterIP, NodePort, LoadBalancer, and ExternalName services.
5. How do labels, selectors, and annotations work?
6. How do ConfigMaps and Secrets differ?
7. Explain readiness, liveness, and startup probes.
8. How do resource requests and limits affect scheduling and runtime?
9. What are namespaces, service accounts, RBAC roles, and bindings?
10. How does Kubernetes service discovery and DNS work?
11. How do rolling updates and rollbacks work?
12. Why might a Pod remain Pending?
13. How do you troubleshoot `CrashLoopBackOff`?
14. How do you troubleshoot `ImagePullBackOff`?
15. How do PersistentVolumes, PersistentVolumeClaims, and StorageClasses work?
16. Explain taints, tolerations, node affinity, and pod affinity.
17. How do autoscaling mechanisms such as HPA, VPA, and Cluster Autoscaler differ?
18. What are PodDisruptionBudgets and topology-spread constraints?
19. How would you secure a production Kubernetes cluster?
20. How would you troubleshoot an application that is healthy inside its Pod but inaccessible externally?

## Helm

1. What problem does Helm solve?
2. Explain charts, releases, repositories, and chart dependencies.
3. Describe the standard structure of a Helm chart.
4. How do templates and `values.yaml` work?
5. How do you override values for different environments?
6. What are built-in objects such as `.Values`, `.Release`, and `.Chart`?
7. How do named templates and helper files work?
8. Explain Helm functions and pipelines.
9. How do `if`, `with`, and `range` work in templates?
10. What is the difference between `helm install` and `helm upgrade --install`?
11. How do Helm release revisions and rollbacks work?
12. What do `helm template`, `helm lint`, and `--dry-run` do?
13. What are Helm hooks, and when should they be used?
14. How are CRDs managed by Helm?
15. How do you manage chart dependencies?
16. How should secrets be managed with Helm?
17. What happens if a Helm upgrade fails?
18. How do `--atomic`, `--wait`, and `--timeout` affect deployments?
19. How would you design reusable charts for multiple applications?
20. How do you debug malformed or incorrectly rendered Helm manifests?

## Argo CD

1. What is GitOps, and how does Argo CD implement it?
2. Explain Argo CD’s major components.
3. What are Applications, Projects, repositories, and clusters?
4. What is the difference between automated and manual synchronization?
5. What do Synced, OutOfSync, Healthy, and Degraded mean?
6. How does Argo CD detect configuration drift?
7. What are prune and self-heal?
8. How do sync waves, phases, and hooks control deployment order?
9. What are ApplicationSets, and which generators have you used?
10. How does Argo CD deploy Helm charts or Kustomize overlays?
11. How do you manage multiple clusters and environments?
12. How do you restrict teams using AppProjects and RBAC?
13. How should secrets be handled in a GitOps repository?
14. How do you roll back an Argo CD deployment?
15. What happens if someone manually edits a managed Kubernetes resource?
16. How do you troubleshoot an application stuck OutOfSync?
17. How do you troubleshoot a failed or continuously running sync?
18. How do Argo CD Image Updater and Git-based image updates work?
19. How would you implement promotion from development to production?
20. How would you operate Argo CD for high availability and disaster recovery?

## Istio

1. What is a service mesh, and why would you introduce one?
2. Explain Istio’s control plane and data plane.
3. What roles do Istiod and Envoy proxies perform?
4. How is sidecar injection enabled and verified?
5. Explain VirtualServices and DestinationRules.
6. How do Gateways differ from Kubernetes Ingress?
7. How do traffic splitting and canary deployments work?
8. How do retries, timeouts, and circuit breakers work?
9. What are service subsets, and how are they selected?
10. How does mutual TLS work in Istio?
11. Compare `PERMISSIVE`, `STRICT`, and `DISABLE` peer-authentication modes.
12. How do AuthorizationPolicies control service-to-service access?
13. How does Istio provide metrics, logs, and distributed tracing?
14. What are sidecar resource and latency overheads?
15. How do you troubleshoot `503` or upstream connection errors?
16. How do you troubleshoot a service that stops working after sidecar injection?
17. How do ServiceEntries provide access to external services?
18. What are egress gateways, and why would you use them?
19. How do locality-aware routing and outlier detection work?
20. What are ambient mesh and sidecar mode, and how do their trade-offs differ?