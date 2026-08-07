# Top 50 DevOps Interview Questions and Answers

> A practical interview-preparation guide covering cloud platforms, infrastructure automation, containers, Kubernetes, CI/CD, scripting, artifact management, and DevSecOps.

---

# Azure Interview Questions and Answers

## 1. What is Microsoft Azure?

Azure is Microsoft’s cloud platform. It provides compute, storage, networking, databases, identity, security, analytics, AI, and DevOps services through Microsoft-managed data centers.

## 2. What are Azure regions and Availability Zones?

A region is a geographical area containing one or more data centers. Availability Zones are physically separate data-center locations within a region, with independent power, cooling, and networking. Deploying across zones reduces the risk of a single data-center failure.

## 3. What is an Azure resource group?

A resource group is a logical container for related Azure resources. It defines a management boundary for deployment, access control, policy, monitoring, tagging, and lifecycle operations.

## 4. Can an Azure resource belong to multiple resource groups?

No. A resource belongs to exactly one resource group at a time, although it can communicate with resources in other resource groups.

## 5. What is an Azure subscription?

A subscription is a billing, quota, access-control, and management boundary. Organizations commonly use separate subscriptions for production, non-production, shared services, or different business units.

## 6. Explain the Azure management hierarchy.

The hierarchy is:

`Management Group → Subscription → Resource Group → Resource`

Policies and role assignments applied at a higher level can be inherited by lower levels.

## 7. What is Azure Resource Manager?

Azure Resource Manager, or ARM, is Azure’s management and deployment layer. It processes requests from the portal, CLI, PowerShell, SDKs, Bicep, and ARM templates and provides consistent authentication, authorization, tagging, locking, and policy enforcement.

## 8. What is the difference between ARM templates and Bicep?

ARM templates use JSON, while Bicep is a concise declarative language that compiles into ARM templates. Bicep is generally easier to read, reuse, and maintain.

## 9. What is an Azure Virtual Network?

A Virtual Network, or VNet, is a logically isolated network in Azure. It contains address spaces, subnets, route tables, security controls, private endpoints, and connectivity to other networks.

## 10. What is the difference between a VNet and a subnet?

A VNet defines the complete private IP address space. A subnet divides that space into smaller network segments so workloads can be separated and controlled independently.

## 11. What is VNet peering?

VNet peering connects two Azure virtual networks through Microsoft’s private backbone. Resources communicate using private IP addresses without requiring a public internet connection.

## 12. Is VNet peering transitive?

No. If VNet A is peered with B and B is peered with C, A does not automatically communicate with C. Transit must be implemented using additional peerings, a hub router, Azure Firewall, or Virtual WAN.

## 13. What is a Network Security Group?

A Network Security Group filters inbound and outbound network traffic using priority-based allow and deny rules. It can be associated with a subnet or network interface.

## 14. How are NSG rules evaluated?

Rules are processed in ascending priority order. The first matching rule is applied. Lower priority numbers are evaluated first, and processing stops after a match.

## 15. What is Azure Application Security Group?

An Application Security Group lets administrators group virtual-machine network interfaces by application role. NSG rules can then reference groups such as `web`, `api`, or `database` instead of individual IP addresses.

## 16. Compare Azure Load Balancer and Application Gateway.

Azure Load Balancer operates at Layer 4 and routes TCP or UDP traffic. Application Gateway operates at Layer 7 and supports HTTP-aware routing, TLS termination, path-based routing, session affinity, and Web Application Firewall.

## 17. What is Azure Front Door?

Azure Front Door is a global Layer 7 entry point for web applications. It provides global routing, acceleration, TLS termination, health probes, caching, failover, and optional web application firewall protection.

## 18. What is Azure Traffic Manager?

Traffic Manager is a DNS-based global traffic-routing service. It directs users to endpoints based on methods such as priority, performance, weighted distribution, geography, or subnet.

## 19. What is Azure Private Link?

Private Link exposes a supported Azure service through a private endpoint in a VNet. Clients connect using a private IP address instead of accessing the service’s public endpoint.

## 20. What is an Azure service endpoint?

A service endpoint extends a subnet’s identity to a supported Azure service over the Azure backbone. The service still uses its public endpoint, unlike Private Link, which assigns it a private endpoint in the VNet.

## 21. Compare VPN Gateway and ExpressRoute.

VPN Gateway creates encrypted connections over the public internet. ExpressRoute provides a private connection through a connectivity provider and normally offers more predictable bandwidth and latency.

## 22. What is Azure Virtual WAN?

Virtual WAN is a managed networking service for connecting branches, VNets, VPN users, and ExpressRoute circuits through Microsoft-managed virtual hubs.

## 23. What is a user-defined route?

A user-defined route overrides or supplements Azure’s system routes. It can direct traffic to a virtual appliance, firewall, VPN gateway, virtual network gateway, internet, or a blackhole destination.

## 24. What are Azure virtual-machine availability sets?

An availability set distributes virtual machines across fault domains and update domains. It reduces the chance that planned maintenance or a localized hardware failure affects every instance.

## 25. Compare Availability Sets and Availability Zones.

Availability Sets distribute VMs within one data-center location. Availability Zones distribute workloads across physically separate data-center locations within a region and provide stronger isolation.

## 26. What are Virtual Machine Scale Sets?

VM Scale Sets deploy and manage a group of load-balanced virtual machines. They support consistent configuration, automatic scaling, health monitoring, and rolling upgrades.

## 27. How would you troubleshoot an unreachable Azure VM?

Check the VM power state, NIC and private IP, NSG effective rules, route tables, public IP or load balancer, guest firewall, service status, DNS, Network Watcher diagnostics, serial console, and boot diagnostics.

## 28. What is Azure App Service?

App Service is a managed platform for hosting web applications, REST APIs, and background services. Azure manages the underlying operating system, patching, load balancing, and much of the runtime infrastructure.

## 29. What are App Service deployment slots?

Deployment slots are separate live environments such as staging and production. A validated staging version can be swapped into production, supporting low-downtime releases and rapid rollback.

## 30. Compare Azure Container Instances and Azure Kubernetes Service.

Container Instances runs containers without managing an orchestrator and is suitable for simple or short-lived workloads. AKS provides managed Kubernetes for applications requiring orchestration, scaling, service discovery, policies, and complex deployments.

## 31. What is AKS?

Azure Kubernetes Service is Microsoft’s managed Kubernetes offering. Azure manages the control plane, while the customer configures and operates node pools, workloads, networking, access, policies, and application security.

## 32. What is Azure Functions?

Azure Functions is a serverless, event-driven compute service. It runs code in response to triggers such as HTTP requests, schedules, queues, event streams, or storage events.

## 33. What is an Azure Storage account?

A Storage account is a namespace that provides access to services such as Blob, Files, Queues, and Tables. It defines settings for performance, redundancy, networking, encryption, and data protection.

## 34. Explain Azure Storage redundancy options.

Common options include:

- LRS: Copies data within one data-center location.
- ZRS: Copies data across Availability Zones.
- GRS: Replicates data to a secondary region.
- RA-GRS: Adds read access to the secondary region.
- GZRS: Combines zone redundancy with regional replication.
- RA-GZRS: Adds read access to the secondary GZRS copy.

## 35. What are Azure Blob Storage access tiers?

The principal tiers are Hot, Cool, Cold, and Archive. Hot is optimized for frequent access, while progressively colder tiers reduce storage cost but increase access costs and retrieval restrictions.

## 36. Compare managed disks and Azure Files.

Managed disks provide block storage, primarily for virtual-machine disks. Azure Files provides managed SMB or NFS file shares that can be mounted by multiple supported clients.

## 37. What is Microsoft Entra ID?

Microsoft Entra ID, formerly Azure Active Directory, is Microsoft’s cloud identity and access-management service. It manages users, groups, applications, service principals, managed identities, authentication, and conditional access.

## 38. What is Azure RBAC?

Azure role-based access control determines who can perform which actions at a given scope. A role assignment combines a security principal, a role definition, and a scope.

## 39. What is the difference between Azure RBAC and Entra ID roles?

Azure RBAC controls access to Azure resources, such as virtual machines and storage accounts. Entra ID roles control identity-directory operations, such as managing users, groups, and application registrations.

## 40. What is a service principal?

A service principal is an application identity within an Entra tenant. Applications, automation tools, and CI/CD pipelines use it to authenticate and access authorized resources.

## 41. What is a managed identity?

A managed identity is an Azure-managed identity for a supported resource. It lets an application obtain Entra tokens without storing client secrets or credentials in code.

## 42. Compare system-assigned and user-assigned managed identities.

A system-assigned identity is tied to one resource and is deleted with it. A user-assigned identity is an independent Azure resource that can be attached to multiple supported resources.

## 43. What is Azure Key Vault?

Key Vault securely stores and controls access to secrets, cryptographic keys, and certificates. Applications should retrieve values at runtime using managed identities whenever possible.

## 44. What is Azure Policy?

Azure Policy evaluates resources against organizational rules. It can audit noncompliance, deny deployments, modify properties, deploy required configuration, or remediate existing resources.

## 45. What is the difference between Azure Policy and RBAC?

RBAC controls who can perform an action. Azure Policy controls which resource configurations are permitted or required, regardless of who initiates the deployment.

## 46. What are Azure Monitor, Log Analytics, and Application Insights?

Azure Monitor is the overall monitoring platform. Log Analytics stores and queries logs using KQL. Application Insights provides application performance monitoring, dependency tracking, request telemetry, exceptions, and distributed tracing.

## 47. How would you reduce Azure costs?

Use appropriate resource sizing, autoscaling, reservations or savings plans, Spot VMs where suitable, storage lifecycle policies, scheduled shutdowns, cost budgets, resource cleanup, tagging, and Azure Advisor recommendations.

## 48. How would you design a highly available Azure application?

Deploy instances across Availability Zones, place them behind a zone-redundant load balancer or Application Gateway, use a resilient database configuration, remove single points of failure, implement health probes, and use regional disaster recovery where required.

## 49. How would you implement disaster recovery in Azure?

Define RTO and RPO, select a secondary region, replicate applications and data, maintain infrastructure as code, use services such as Azure Site Recovery where appropriate, automate failover, document dependencies, and regularly test recovery procedures.

## 50. Describe a secure Azure landing zone.

A landing zone normally includes a management-group and subscription hierarchy, centralized identity, policy guardrails, network topology, logging, security monitoring, cost controls, resource naming and tagging, deployment automation, and separated production and non-production environments.

---

<!-- Next section: AWS Interview Questions and Answers -->