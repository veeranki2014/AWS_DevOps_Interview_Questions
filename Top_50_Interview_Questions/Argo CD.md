```markdown
# Argo CD Interview Questions and Answers

## 1. What is Argo CD?

Argo CD is a declarative continuous-delivery tool for Kubernetes. It monitors application definitions in Git and reconciles Kubernetes clusters with the desired state stored in those repositories.

## 2. What is GitOps?

GitOps is an operating model in which Git contains the reviewed desired state of a system. Automated controllers compare that desired state with the running environment and reconcile approved changes.

## 3. What are the primary benefits of GitOps?

Benefits include version-controlled configuration, pull-request review, traceability, reproducibility, automated drift detection, easier rollback, and reduced dependence on manually executed deployment commands.

## 4. What are the main Argo CD components?

Important components include:

- API server
- Repository server
- Application controller
- Redis
- Dex when configured
- ApplicationSet controller
- Supporting identity, notification, and extension components

## 5. What does the Argo CD API server do?

It provides APIs used by the web UI, CLI, automation, authentication, RBAC enforcement, application operations, repository configuration, and cluster management.

## 6. What does the repository server do?

The repository server retrieves source repositories and generates Kubernetes manifests using supported tools such as plain YAML, Helm, Kustomize, or configured plugins.

## 7. What does the application controller do?

The application controller compares desired manifests with live cluster resources, calculates synchronization and health states, and performs reconciliation operations.

## 8. What is an Argo CD Application?

An Application defines a source repository and revision, a source path or chart, a destination cluster and namespace, and synchronization behavior.

## 9. What is an AppProject?

An AppProject groups Applications and defines boundaries such as permitted source repositories, deployment destinations, cluster-scoped resources, namespace-scoped resources, and project roles.

## 10. What is an ApplicationSet?

An ApplicationSet generates multiple Argo CD Applications from a template and one or more generators. It is useful for multi-cluster, multi-environment, or multi-tenant deployments.

## 11. Which ApplicationSet generators are commonly used?

Common generators include:

- List
- Cluster
- Git directory
- Git file
- Matrix
- Merge
- Pull request
- SCM provider
- Cluster decision resource

## 12. What is the difference between push-based and pull-based deployment?

In push-based deployment, a pipeline sends changes directly to the cluster. In pull-based GitOps, an in-cluster controller retrieves approved desired state and reconciles it.

## 13. What does `Synced` mean in Argo CD?

`Synced` means the tracked live resources match the desired manifests generated from the configured source and revision.

## 14. What does `OutOfSync` mean?

`OutOfSync` means Argo CD detected a difference between the desired configuration and live cluster state, or a desired resource has not yet been created.

## 15. What does `Healthy` mean?

`Healthy` means Argo CD’s health assessment considers the application resources operational. Health and synchronization are independent—a resource can be synchronized but unhealthy.

## 16. What does `Degraded` mean?

`Degraded` indicates that one or more resources are failing or not progressing as expected, such as a Deployment with unavailable replicas or a failed Job.

## 17. What is manual synchronization?

With manual synchronization, Argo CD detects differences but waits for a user or automation process to initiate the sync operation.

## 18. What is automated synchronization?

Automated synchronization allows Argo CD to apply Git changes without a manual sync request when the application becomes out of sync.

## 19. What is self-healing?

Self-healing allows automated synchronization to restore resources when live cluster state is manually changed away from the desired Git state.

## 20. What is pruning?

Pruning deletes a managed cluster resource when it no longer exists in the desired source. It should be enabled carefully because an incorrect Git change can otherwise cause unintended deletion.

## 21. What is the difference between refresh and sync?

Refresh recalculates desired and live state to detect differences. Sync applies the desired manifests to the destination cluster.

## 22. What is Argo CD’s reconciliation loop?

The controller periodically and eventfully compares desired Git state with live resources. It updates status and, when configured, performs synchronization to correct differences.

## 23. How does Argo CD track managed resources?

Argo CD uses resource-tracking metadata, commonly labels or annotations, to associate live Kubernetes resources with an Application.

## 24. How does Argo CD work with Helm?

Argo CD uses Helm to render charts into Kubernetes manifests. Argo CD then manages the rendered resources; it does not use Helm’s release lifecycle in the same way as `helm install`.

## 25. How does Argo CD work with Kustomize?

Argo CD runs Kustomize against the configured source path and applies the generated manifests to the target cluster.

## 26. What is a config-management plugin?

A config-management plugin allows Argo CD to generate manifests using a custom tool or workflow not covered by built-in source types.

## 27. What are sync phases?

Argo CD defines phases such as:

- `PreSync`
- `Sync`
- `PostSync`
- `SyncFail`
- `PostDelete`

They organize hook execution around synchronization.

## 28. What are sync waves?

Sync waves control the order in which resources are applied within a phase. Lower-numbered waves are processed before higher-numbered waves.

Example:

```yaml
argocd.argoproj.io/sync-wave: "-1"
```

## 29. What are Argo CD resource hooks?

Hooks are annotated Kubernetes resources, frequently Jobs, that run during synchronization phases. They can perform migrations, validation, notifications, or cleanup.

## 30. How can database migrations be handled?

Use a carefully designed `PreSync` or `Sync` Job, make migrations backward-compatible and idempotent, define failure behavior, and avoid assuming that application rollback automatically reverses schema changes.

## 31. What are sync options?

Sync options modify synchronization behavior. Examples include server-side apply, namespace creation, pruning controls, resource replacement, validation behavior, and selective application.

## 32. What does `CreateNamespace=true` do?

It instructs Argo CD to create the destination namespace when it does not exist. Namespace metadata may require additional managed-namespace configuration.

## 33. What is selective synchronization?

Selective synchronization applies only selected resources rather than the complete Application. It is useful operationally but should be used carefully because it can bypass normal ordering or hooks.

## 34. What happens if someone manually edits an Argo CD-managed resource?

Argo CD detects drift during refresh. With self-healing enabled, it reapplies the Git state. Without self-healing, the Application remains out of sync until someone synchronizes or changes Git.

## 35. How do you ignore an expected difference?

Configure `ignoreDifferences` for a precise resource, field, JSON pointer, JQ expression, or manager. Keep exclusions narrow so genuine drift remains visible.

## 36. Why might a resource remain OutOfSync after synchronization?

Possible causes include mutating admission webhooks, controllers changing fields, defaulted values, generated data, incorrect ignore rules, failed application, immutable fields, or conflicting resource managers.

## 37. How do you troubleshoot an OutOfSync Application?

Compare desired and live manifests, inspect the diff, refresh the Application, review sync results and events, check mutating controllers, verify the source revision, and examine ignored differences.

## 38. How do you troubleshoot a failed sync?

Inspect the operation message, resource results, hooks, Kubernetes events, admission denials, RBAC, immutable-field conflicts, missing CRDs, namespace restrictions, and resource health.

## 39. How can an Argo CD deployment be rolled back?

Prefer reverting the Git commit so Git remains the source of truth. Argo CD also supports synchronization to an earlier revision, but the repository should ultimately reflect the intended state.

## 40. How should environment promotion be implemented?

Build an immutable artifact once, then update its version or digest in environment-specific Git configuration through reviewed pull requests. Promote the same artifact instead of rebuilding it per environment.

## 41. How are multiple environments organized?

Common designs use separate directories, overlays, branches, or repositories. The design should preserve clear ownership, controlled promotion, minimal duplication, and strong production protection.

## 42. How are multiple clusters registered?

Argo CD requires credentials and connection information for destination clusters. Registration can be performed declaratively or administratively, with access restricted to required namespaces and resources.

## 43. How does Argo CD RBAC work?

Argo CD maps users and groups to roles and policies. Policies determine allowed actions on applications, projects, clusters, repositories, logs, and other Argo CD resources.

## 44. How do AppProjects support multi-tenancy?

Projects restrict where Applications may deploy, which sources they may use, and which resource types they may create. Project roles can then grant teams limited access within those boundaries.

## 45. How should repository credentials be managed?

Store credentials in protected Kubernetes Secrets or approved external secret systems, scope them narrowly, rotate them, and prefer deploy keys, GitHub Apps, or short-lived credentials where supported.

## 46. How should application secrets be stored in GitOps?

Do not commit plaintext production secrets. Use an external secrets operator, secret-store CSI integration, encrypted secret format, or another approved system with controlled decryption.

## 47. What is the App-of-Apps pattern?

A parent Argo CD Application manages manifests that define child Applications. It bootstraps groups of applications but requires careful ownership, ordering, deletion, and repository-boundary decisions.

## 48. What is the ApplicationSet alternative to App-of-Apps?

ApplicationSet generates Applications using data-driven templates. It is often preferable when many Applications follow a consistent pattern across clusters, directories, or environments.

## 49. How would you operate Argo CD for high availability?

Run supported components with appropriate replicas, distribute them across failure domains, protect Redis and repository access, monitor queues and reconciliation, back up declarative configuration, and maintain a tested recovery procedure.

## 50. What should be monitored in Argo CD?

Monitor component availability, reconciliation latency, sync failures, unhealthy and OutOfSync applications, repository errors, cluster connectivity, API latency, controller queues, certificate expiry, authentication failures, and notification delivery.

