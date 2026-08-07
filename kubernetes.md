```markdown
# Kubernetes Interview Questions and Answers

## 1. What is Kubernetes?

Kubernetes is an open-source container-orchestration platform. It automates application deployment, scheduling, scaling, networking, service discovery, configuration, and recovery across a cluster.

## 2. What is a Kubernetes cluster?

A cluster consists of a control plane and worker nodes. The control plane manages desired state, while worker nodes run containerized workloads.

## 3. What are the main Kubernetes control-plane components?

The principal components are:

- `kube-apiserver`
- `etcd`
- `kube-scheduler`
- `kube-controller-manager`
- An optional cloud-controller manager

## 4. What does `kube-apiserver` do?

The API server exposes the Kubernetes API. It authenticates and authorizes requests, performs admission processing and validation, and persists accepted state through `etcd`.

## 5. What is `etcd`?

`etcd` is a distributed key-value store containing Kubernetes cluster state. It requires restricted access, encryption, reliable backups, and tested recovery procedures.

## 6. What does `kube-scheduler` do?

The scheduler assigns unscheduled Pods to suitable nodes. It considers resource requirements, constraints, affinity, taints, topology, and other scheduling rules.

## 7. What does `kube-controller-manager` do?

It runs reconciliation controllers. Each controller watches cluster state and attempts to move actual conditions toward the declared desired state.

## 8. What components normally run on worker nodes?

Worker nodes normally run:

- `kubelet`
- A container runtime
- `kube-proxy` or another networking implementation
- Pods assigned to the node

## 9. What does the kubelet do?

The kubelet ensures that containers described in assigned Pod specifications are running and healthy. It communicates with the runtime and reports node and Pod status.

## 10. What happens when `kubectl apply` is executed?

`kubectl` sends the desired configuration to the API server. After authentication, authorization, admission, and persistence, controllers reconcile resources, the scheduler assigns Pods, and kubelets start containers.

## 11. What is a Pod?

A Pod is Kubernetes’ smallest deployable unit. It contains one or more tightly coupled containers that share networking, volumes, and lifecycle context.

## 12. Why should most Pods not be created directly?

A directly created Pod is not automatically replaced after deletion or node failure. A controller such as a Deployment, StatefulSet, Job, or DaemonSet should usually manage it.

## 13. What is a ReplicaSet?

A ReplicaSet maintains a specified number of matching Pod replicas. Deployments usually manage ReplicaSets instead of users managing them directly.

## 14. What is a Deployment?

A Deployment manages stateless application replicas through ReplicaSets. It supports declarative rolling updates, scaling, pause and resume operations, and revision rollback.

## 15. What is a StatefulSet?

A StatefulSet manages applications that need stable Pod identities, ordered operations, or persistent storage associated with individual replicas.

## 16. What is a DaemonSet?

A DaemonSet ensures that matching nodes run a copy of a Pod. Common use cases include log collectors, monitoring agents, storage components, and networking agents.

## 17. What is the difference between a Job and a CronJob?

A Job runs Pods until a task completes successfully. A CronJob creates Jobs according to a schedule.

## 18. What is a Kubernetes Service?

A Service provides a stable network identity and logical endpoint for a changing set of Pods selected by labels.

## 19. What are the principal Service types?

- `ClusterIP`: Internal cluster access.
- `NodePort`: Exposes a port on participating nodes.
- `LoadBalancer`: Requests an external load balancer from a supported integration.
- `ExternalName`: Returns a configured DNS alias.

## 20. What is a headless Service?

A headless Service uses `clusterIP: None`. Instead of providing one virtual IP, DNS can return individual backend Pod addresses, which is useful for StatefulSets and client-side discovery.

## 21. What is Kubernetes Ingress?

Ingress is an API for HTTP and HTTPS routing into cluster services. It requires an Ingress controller to implement the routing behavior.

## 22. How is Gateway API different from Ingress?

Gateway API provides more expressive, role-oriented, and extensible traffic-management resources. It separates infrastructure ownership from application routing more clearly than the traditional Ingress API.

## 23. How does Kubernetes service discovery work?

The cluster DNS service creates DNS records for Services and supported Pod patterns. Applications discover a Service using names such as:

```text
service-name.namespace-name.svc.cluster.local
```

## 24. What are labels and selectors?

Labels are key-value metadata used to organize resources. Selectors identify resources with matching labels and connect objects such as Services, Deployments, and Pods.

## 25. What are annotations?

Annotations store non-identifying metadata such as tool configuration, ownership details, checksums, or external-system information. Selectors do not use annotations.

## 26. What is a namespace?

A namespace provides logical separation and naming scope inside a cluster. It can be combined with RBAC, quotas, policies, and network controls, but it is not automatically a complete security boundary.

## 27. What is a ConfigMap?

A ConfigMap stores non-sensitive configuration data as key-value entries or files. Pods can consume it through environment variables, command arguments, or mounted volumes.

## 28. What is a Kubernetes Secret?

A Secret stores sensitive data for use by workloads. Base64 encoding is not encryption, so access must be restricted and encryption at rest should be enabled.

## 29. How should Kubernetes secrets be managed securely?

Use an external secret manager or controlled secret-delivery mechanism, enable encryption at rest, restrict RBAC, avoid exposing secrets through logs or environment dumps, and rotate credentials.

## 30. Compare liveness, readiness, and startup probes.

- Liveness determines whether a container should be restarted.
- Readiness determines whether it should receive Service traffic.
- Startup protects slow-starting containers from liveness checks until initialization succeeds.

## 31. What are resource requests and limits?

A request is the resource amount considered by the scheduler and generally reserved for placement decisions. A limit is the maximum runtime usage allowed or enforced for a resource.

## 32. What are Kubernetes QoS classes?

Pods are classified as `Guaranteed`, `Burstable`, or `BestEffort` based on resource requests and limits. The class influences behavior under node resource pressure.

## 33. What happens when a container exceeds its memory limit?

The container process may be terminated by the kernel for exceeding the cgroup memory limit. Kubernetes can then restart it according to the Pod’s restart policy.

## 34. What happens when a container exceeds its CPU limit?

CPU is generally throttled rather than terminated. Excessive throttling can increase response time and reduce throughput.

## 35. Why might a Pod remain Pending?

Common causes include insufficient resources, incompatible node selectors or affinity, untolerated taints, unbound storage, quota restrictions, scheduling gates, or unavailable nodes.

## 36. What is `CrashLoopBackOff`?

It means a container repeatedly starts and exits, and Kubernetes is delaying subsequent restart attempts. It is a symptom rather than the root cause.

## 37. How do you troubleshoot `CrashLoopBackOff`?

Inspect Pod events, current and previous logs, exit codes, probes, commands, configuration, secrets, permissions, dependencies, and resource limits:

```bash
kubectl describe pod POD
kubectl logs POD
kubectl logs POD --previous
```

## 38. What is `ImagePullBackOff`?

It indicates repeated failure to retrieve a container image. Possible causes include an incorrect image reference, missing tag, registry authentication failure, network issues, rate limits, or architecture incompatibility.

## 39. What are PersistentVolumes and PersistentVolumeClaims?

A PersistentVolume represents cluster storage. A PersistentVolumeClaim is a workload’s request for storage with particular capacity and access characteristics.

## 40. What is a StorageClass?

A StorageClass describes a storage type and provisioner. It commonly enables dynamic volume provisioning when a compatible claim is created.

## 41. What are taints and tolerations?

A taint discourages or prevents Pods from scheduling on a node. A matching toleration allows a Pod to be considered for that node but does not guarantee placement.

## 42. What are node affinity and Pod affinity?

Node affinity places Pods according to node labels. Pod affinity and anti-affinity place Pods relative to other Pods across defined topology domains.

## 43. What are topology-spread constraints?

They distribute matching Pods across zones, nodes, or other topology domains to improve availability and avoid concentration.

## 44. What is a PodDisruptionBudget?

A PodDisruptionBudget limits how many replicas of an application may be voluntarily disrupted simultaneously. It does not prevent every involuntary failure.

## 45. How do rolling updates work?

A Deployment gradually creates Pods from a new template and removes old Pods according to `maxSurge` and `maxUnavailable`. Readiness determines whether new Pods are eligible for traffic.

## 46. How do you roll back a Deployment?

Review revision history and undo to a selected revision:

```bash
kubectl rollout history deployment/APP
kubectl rollout undo deployment/APP
```

Rollback does not automatically reverse external changes such as database migrations.

## 47. Compare HPA, VPA, and Cluster Autoscaler.

- HPA changes workload replica counts.
- VPA recommends or changes Pod resource requests.
- Cluster Autoscaler changes the number of worker nodes according to scheduling demand and utilization conditions.

## 48. What are service accounts and RBAC?

Service accounts provide workload identities. RBAC uses Roles or ClusterRoles and bindings to authorize operations for users, groups, and service accounts.

## 49. How would you secure a production Kubernetes cluster?

Use least-privilege RBAC, workload identity, network policies, Pod Security controls, encrypted secrets, image verification and scanning, private endpoints where appropriate, audit logs, patched nodes, admission policies, and protected backups.

## 50. A Pod is healthy but the application is inaccessible. How do you troubleshoot it?

Verify application bindings, container ports, readiness, Service selectors and endpoints, target ports, DNS, network policies, Ingress or Gateway configuration, load-balancer health, firewalls, and the complete request path:

```bash
kubectl get pod -o wide
kubectl get service,endpoints
kubectl describe service SERVICE
kubectl get ingress
kubectl exec POD -- curl localhost:PORT
```

---

<!-- Next section: Helm Interview Questions and Answers -->
```## How secure kubernetes on production environments?
