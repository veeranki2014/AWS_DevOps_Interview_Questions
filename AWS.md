# AWS Interview Questions and Answers

## 1. What is Amazon Web Services?

Amazon Web Services, or AWS, is a cloud-computing platform that provides services for compute, storage, networking, databases, security, analytics, machine learning, monitoring, and application deployment.

## 2. What is the AWS shared responsibility model?

AWS is responsible for security **of** the cloud, including physical facilities, hardware, networking, and managed-service infrastructure. Customers are responsible for security **in** the cloud, including identities, data, configurations, operating systems, and applications, depending on the service used.

## 3. What are AWS Regions and Availability Zones?

A Region is a separate geographic area containing multiple Availability Zones. An Availability Zone consists of one or more isolated data centers with independent power, cooling, and networking.

## 4. What are Local Zones and edge locations?

Local Zones place selected AWS services closer to a metropolitan area for low-latency workloads. Edge locations run services such as CloudFront and Route 53 near users to improve content delivery and request performance.

## 5. What is Amazon EC2?

Amazon Elastic Compute Cloud provides resizable virtual machines called instances. Customers select an instance family, operating system, network, storage, security groups, purchasing model, and scaling configuration.

## 6. What are the main EC2 purchasing options?

Common options include:

- On-Demand Instances for flexible usage.
- Savings Plans or Reserved Instances for predictable workloads.
- Spot Instances for interruptible workloads.
- Dedicated Hosts or Dedicated Instances for isolation or licensing requirements.

## 7. How do you select an EC2 instance type?

Consider CPU, memory, network bandwidth, storage performance, architecture, accelerator requirements, workload behavior, availability, licensing, and cost. Validate the choice using monitoring and load testing.

## 8. What is an Amazon Machine Image?

An Amazon Machine Image, or AMI, is a template used to launch EC2 instances. It includes an operating-system image, block-device mappings, permissions, and launch-related metadata.

## 9. What is EC2 user data?

User data is a script or cloud-init configuration supplied at instance launch. It is commonly used to install packages, configure services, or register the instance with another system.

## 10. What is an EC2 Auto Scaling group?

An Auto Scaling group maintains a desired number of EC2 instances between configured minimum and maximum values. It can replace unhealthy instances and adjust capacity based on policies, schedules, or demand.

## 11. Compare target tracking, step, and scheduled scaling.

Target tracking maintains a metric near a target value. Step scaling changes capacity by different amounts depending on alarm severity. Scheduled scaling changes capacity at predefined times.

## 12. Compare Application, Network, and Gateway Load Balancers.

- Application Load Balancer operates at Layer 7 for HTTP and HTTPS traffic.
- Network Load Balancer operates at Layer 4 for high-performance TCP, UDP, and TLS traffic.
- Gateway Load Balancer helps deploy and scale third-party network appliances.

## 13. What is a target group?

A target group contains destinations such as EC2 instances, IP addresses, or Lambda functions. A load balancer forwards matching requests to healthy targets in the group.

## 14. What is an AWS VPC?

A Virtual Private Cloud is an isolated virtual network. It contains an IP address range, subnets, route tables, gateways, endpoints, security controls, and other networking resources.

## 15. What is the difference between public and private subnets?

A public subnet has a route to an internet gateway. A private subnet does not provide direct internet routing to its resources, although outbound connectivity may be supplied through a NAT device.

## 16. What is an internet gateway?

An internet gateway is a horizontally scaled VPC component that enables communication between a VPC and the internet. A route and appropriate public addressing and security rules are also required.

## 17. What is a NAT gateway?

A NAT gateway allows resources in private subnets to initiate outbound connections while preventing unsolicited inbound internet connections. It is normally deployed in a public subnet with an Elastic IP address.

## 18. Why should NAT gateways be deployed per Availability Zone?

Routing private subnets through a NAT gateway in another Availability Zone introduces a cross-zone dependency and additional data-transfer costs. A NAT gateway per zone improves resilience and traffic locality.

## 19. Compare security groups and network ACLs.

Security groups are stateful and apply to network interfaces. Network ACLs are stateless and apply at the subnet boundary. Security-group return traffic is automatically allowed, while NACL return traffic must be explicitly permitted.

## 20. What is VPC peering?

VPC peering creates private IP connectivity between two VPCs. It is non-transitive, so one peering connection cannot be used as a router between additional VPCs.

## 21. What is AWS Transit Gateway?

Transit Gateway is a regional networking hub that connects multiple VPCs, VPNs, and supported network attachments. It simplifies routing compared with maintaining many individual peering connections.

## 22. What is AWS PrivateLink?

PrivateLink provides private access to supported services through interface VPC endpoints. Traffic remains on the AWS network and the consumer does not need public internet connectivity.

## 23. What are gateway and interface VPC endpoints?

Gateway endpoints provide private routing to supported services such as S3 and DynamoDB. Interface endpoints create private network interfaces for services powered by AWS PrivateLink.

## 24. How do you troubleshoot an unreachable EC2 application?

Check instance health, service status, listening ports, security groups, NACLs, route tables, subnet routing, public or private addressing, load-balancer health checks, DNS, host firewall, and VPC Flow Logs.

## 25. What is AWS IAM?

Identity and Access Management controls authentication and authorization in AWS. It manages users, groups, roles, policies, identity providers, and account-level access settings.

## 26. Compare IAM users and IAM roles.

An IAM user represents a long-term identity with optional credentials. A role provides temporary credentials and can be assumed by trusted users, applications, AWS services, or external identities.

## 27. What is the difference between identity-based and resource-based policies?

Identity-based policies are attached to users, groups, or roles. Resource-based policies are attached to resources and specify which principals can access them.

## 28. What is the principle of least privilege?

Least privilege means granting only the permissions required to perform an approved task, at the narrowest practical resource scope and for the necessary duration.

## 29. How should an EC2 instance access an S3 bucket?

Attach an IAM role to an instance profile and grant it the required S3 permissions. The application obtains temporary credentials automatically rather than storing access keys.

## 30. What is AWS Organizations?

AWS Organizations centrally manages multiple AWS accounts. It supports consolidated billing, organizational units, service control policies, account creation, and governance.

## 31. What is a service control policy?

A service control policy defines the maximum available permissions for accounts or organizational units. It does not grant permissions by itself; IAM permissions are still required.

## 32. What is Amazon S3?

Amazon Simple Storage Service is an object-storage service. It stores objects in buckets and supports versioning, lifecycle management, encryption, replication, access policies, and multiple storage classes.

## 33. What consistency model does Amazon S3 provide?

Amazon S3 provides strong read-after-write consistency for object PUT and DELETE operations. A successful write is immediately visible to subsequent supported reads and listings.

## 34. How do you secure an S3 bucket?

Enable Block Public Access, use least-privilege IAM and bucket policies, encrypt data, enforce TLS, enable logging where required, use versioning, and continuously evaluate configuration and access.

## 35. What is S3 versioning?

Versioning preserves multiple versions of an object. It helps recover from accidental modification or deletion, but lifecycle rules may be needed to control storage costs.

## 36. What are S3 lifecycle rules?

Lifecycle rules transition objects between storage classes or expire current versions, noncurrent versions, and incomplete multipart uploads according to defined conditions.

## 37. Compare EBS, EFS, and S3.

- EBS provides block storage mainly for EC2.
- EFS provides shared managed file storage using NFS.
- S3 provides scalable object storage accessed through APIs.

## 38. What are EBS snapshots?

EBS snapshots are incremental, point-in-time backups stored and managed by AWS. They can restore volumes and copy data across supported Regions or accounts.

## 39. Compare RDS Multi-AZ deployments and read replicas.

Multi-AZ improves availability by maintaining a standby for failover. Read replicas create additional readable database copies for scaling reads and may also support disaster-recovery designs.

## 40. What is Amazon Aurora?

Aurora is an AWS-managed relational database compatible with MySQL or PostgreSQL. Its distributed storage architecture is designed for availability, durability, replication, and managed failover.

## 41. Compare RDS and DynamoDB.

RDS provides managed relational databases with SQL, schemas, joins, and transactions. DynamoDB is a managed NoSQL key-value and document database designed for low-latency access at scale.

## 42. Compare Secrets Manager and Systems Manager Parameter Store.

Secrets Manager focuses on secret storage and rotation. Parameter Store manages configuration and secrets using hierarchical parameters. The best choice depends on rotation, integration, throughput, and cost requirements.

## 43. What is AWS KMS?

AWS Key Management Service creates and controls encryption keys. Integrated AWS services use KMS keys to protect data, while access is governed through key policies, IAM policies, and grants.

## 44. Compare CloudWatch, CloudTrail, and AWS Config.

CloudWatch collects metrics, logs, alarms, and operational telemetry. CloudTrail records AWS API activity. AWS Config records resource configuration and evaluates it against rules.

## 45. What are VPC Flow Logs?

VPC Flow Logs capture metadata about accepted and rejected IP traffic for VPCs, subnets, or network interfaces. They help with security analysis and connectivity troubleshooting.

## 46. What is Amazon Route 53?

Route 53 is a managed DNS and domain service. It supports domain registration, DNS records, health checks, private hosted zones, and multiple traffic-routing policies.

## 47. Explain common Route 53 routing policies.

Common policies include simple, weighted, latency-based, failover, geolocation, geoproximity, multivalue-answer, and IP-based routing.

## 48. How would you design a highly available AWS application?

Deploy stateless application instances across multiple Availability Zones, use load balancing and Auto Scaling, store session state externally, select resilient data services, automate recovery, and monitor every critical dependency.

## 49. How would you design disaster recovery on AWS?

Start with RTO and RPO requirements. Select backup-and-restore, pilot-light, warm-standby, or multi-site architecture; replicate data and artifacts; define DNS or routing failover; automate infrastructure; and test recovery regularly.

## 50. How would you reduce AWS costs?

Right-size resources, remove unused assets, use automatic scaling, Savings Plans or reservations for stable usage, Spot Instances for interruptible work, S3 lifecycle policies, cost-allocation tags, budgets, and Cost Explorer recommendations.

---

<!-- Next section: Terraform Interview Questions and Answers -->