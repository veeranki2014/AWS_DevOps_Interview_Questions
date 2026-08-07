# Terraform Interview Questions and Answers

## 1. What is Terraform?

Terraform is an Infrastructure as Code tool created by HashiCorp. It uses declarative configuration files to provision and manage infrastructure through provider APIs.

## 2. What is Infrastructure as Code?

Infrastructure as Code, or IaC, manages infrastructure through version-controlled configuration instead of manual operations. It improves consistency, repeatability, reviewability, automation, and disaster recovery.

## 3. How is Terraform different from configuration-management tools?

Terraform primarily provisions infrastructure resources. Configuration-management tools such as Ansible primarily configure operating systems and applications. They can be used together.

## 4. What is the Terraform workflow?

The standard workflow is:

1. Write or update configuration.
2. Run `terraform fmt` and `terraform validate`.
3. Run `terraform plan`.
4. Review the proposed changes.
5. Run `terraform apply`.
6. Verify and monitor the resulting infrastructure.

## 5. What does `terraform init` do?

It initializes a working directory, installs providers and modules, configures the backend, and prepares Terraform to perform other operations.

## 6. What does `terraform plan` do?

It compares configuration, state, and provider-reported infrastructure and produces an execution plan describing the actions Terraform proposes.

## 7. What does `terraform apply` do?

It executes the proposed infrastructure changes. Terraform calls provider APIs, updates resources, and records the resulting bindings and attributes in state.

## 8. What does `terraform destroy` do?

It creates and executes a plan to delete resources managed by the current Terraform configuration and state. It should be carefully controlled, particularly in shared or production environments.

## 9. What is a Terraform provider?

A provider is a plugin that allows Terraform to communicate with a platform or service API, such as AWS, Azure, Kubernetes, GitHub, or Datadog.

## 10. What is a Terraform resource?

A resource block describes an infrastructure object that Terraform should manage, such as a virtual machine, network, database, DNS record, or repository.

## 11. What is a data source?

A data source reads information without creating the underlying object. It is commonly used to retrieve details about existing infrastructure for use elsewhere in the configuration.

## 12. What is Terraform state?

State records the relationship between Terraform resource addresses and real infrastructure objects. It also stores resource attributes, dependencies, outputs, and other metadata needed for planning.

## 13. Why does Terraform need state?

Cloud APIs do not retain Terraform resource addresses or configuration relationships. State enables Terraform to map configuration to existing objects, detect changes, resolve dependencies, and calculate plans efficiently.

## 14. Why should state be stored remotely?

A remote backend allows teams and automation systems to share consistent state. Depending on the backend, it can also provide locking, access control, encryption, backup, and version recovery.

## 15. How do you configure an AWS remote backend?

A common configuration stores state in an encrypted S3 bucket. State locking should use the locking mechanism supported by the selected Terraform and backend configuration, with appropriate IAM permissions, versioning, and restricted access.

## 16. How do you configure an Azure remote backend?

Use the `azurerm` backend with an Azure Storage account, blob container, and state key. Authentication should use an approved identity such as workload identity or a service principal rather than embedded credentials.

## 17. What is state locking?

State locking prevents multiple Terraform operations from modifying the same state simultaneously. This protects the state from race conditions and conflicting updates.

## 18. What should you do if a state lock remains after a failed operation?

First confirm that no Terraform process is still using the state. Investigate the failed operation and backend. Use `terraform force-unlock` only with the correct lock ID and only after verifying that removing the lock is safe.

## 19. Does marking a variable as sensitive protect it in state?

No. The `sensitive` setting hides values from much of the CLI output, but sensitive values can still exist in state. State must be encrypted and access-controlled.

## 20. How should secrets be handled in Terraform?

Retrieve secrets from an approved secret manager or inject them securely through CI/CD. Avoid hardcoding credentials, restrict state access, mark outputs sensitive, and prefer references or runtime retrieval when possible.

## 21. What is infrastructure drift?

Drift occurs when real infrastructure differs from Terraform configuration or recorded state, often because of manual changes, external automation, or provider-side behavior.

## 22. How do you detect drift?

Run `terraform plan` or a refresh-only plan regularly. The provider reads current infrastructure and Terraform reports differences from the configuration and state.

## 23. How should drift be corrected?

Determine whether the configuration or external change is authoritative. Either apply Terraform to restore the declared state or update/import the configuration and state to represent the approved infrastructure.

## 24. What is a Terraform module?

A module is a reusable collection of Terraform configuration files. Modules encapsulate infrastructure patterns behind input variables and outputs.

## 25. What is the root module?

The root module is the Terraform configuration in the working directory where commands are executed. Any modules it calls are child modules.

## 26. How should a reusable module be designed?

Give it a focused purpose, define typed inputs, perform input validation, expose useful outputs, document behavior, avoid unnecessary provider configuration, and follow semantic versioning.

## 27. How do `count` and `for_each` differ?

`count` creates instances identified by numeric indexes. `for_each` creates instances identified by stable map keys or set values. `for_each` is often safer when individual items may be added or removed.

## 28. Why can removing an item from a `count` list be dangerous?

Resources are identified by numeric position. Removing an earlier item can shift later indexes, causing Terraform to update or replace resources that were not intended to change.

## 29. What are local values?

Local values assign names to reusable expressions within a module. They reduce repetition and make complex expressions easier to understand.

## 30. What are Terraform outputs?

Outputs expose selected values from a module. They can display useful information, provide inputs to other modules, or make values available to automation through machine-readable output.

## 31. What are input-variable validation rules?

Validation rules define conditions that supplied variable values must satisfy. They fail early with a custom message when input is invalid.

## 32. What is a dynamic block?

A dynamic block generates repeated nested blocks from a collection. It is useful when a resource requires multiple optional or data-driven nested configurations.

## 33. How does Terraform determine dependency order?

Terraform builds a dependency graph from references between resources. It creates or updates independent resources concurrently while respecting dependencies.

## 34. What is `depends_on`?

`depends_on` creates an explicit dependency when Terraform cannot infer one from expressions. It should be reserved for real but otherwise hidden dependencies.

## 35. What are implicit dependencies?

An implicit dependency is created when one block references an attribute from another. Terraform automatically uses that relationship to determine operation order.

## 36. What does the `lifecycle` block do?

A resource lifecycle block changes selected resource-management behavior. Common arguments include `create_before_destroy`, `prevent_destroy`, `ignore_changes`, and `replace_triggered_by`.

## 37. What is `create_before_destroy`?

It instructs Terraform to create a replacement before destroying the existing resource when possible. This can reduce downtime but may fail if both objects cannot exist simultaneously.

## 38. What is `prevent_destroy`?

It causes Terraform to reject a plan that would destroy the protected resource while the lifecycle rule remains in configuration. It is an additional safeguard, not a replacement for backups or access controls.

## 39. When should `ignore_changes` be used?

Use it when an attribute is intentionally managed by another system or changes in an accepted provider-controlled way. Excessive use can hide real drift and weaken Infrastructure as Code.

## 40. What are Terraform workspaces?

CLI workspaces maintain multiple state instances for the same configuration. They can be useful for similar temporary environments but often provide insufficient isolation for complex production environments.

## 41. How should development, staging, and production be separated?

Use separate state files and preferably separate accounts, subscriptions, projects, or directories. Apply distinct permissions, variables, pipelines, approvals, and failure boundaries.

## 42. What is `terraform import`?

Import associates an existing infrastructure object with a Terraform resource address. Modern Terraform also supports declarative import blocks for plan-visible and repeatable import workflows.

## 43. Does importing a resource generate its complete configuration?

Import establishes the state association. Configuration must still represent the resource correctly, although supported Terraform workflows may help generate an initial configuration that requires review and refinement.

## 44. What is a `moved` block?

A `moved` block records a resource or module address change. It allows Terraform to migrate the state address without destroying and recreating the underlying object.

## 45. What is a `removed` block?

A `removed` block declares that an object is no longer managed by the configuration. Depending on its lifecycle configuration, Terraform can remove it from state without destroying the real resource.

## 46. What happens if `terraform apply` fails midway?

Successfully completed operations usually remain in the infrastructure and state. Fix the underlying issue, inspect the current state and infrastructure, run a new plan, and continue from the resulting proposal.

## 47. How do you recover a lost or corrupted state file?

Stop all writes, recover a known-good version from backend versioning or backup, compare it with real infrastructure, and reconcile missing objects carefully. Use state commands only after making a backup.

## 48. What validation should Terraform receive in CI?

Typical checks include `terraform fmt -check`, `terraform init`, `terraform validate`, linting, security and policy scanning, module tests, and a reviewed plan generated with controlled credentials.

## 49. How should Terraform be executed in CI/CD?

Use short-lived workload identity, remote state with locking, pinned dependencies, protected environments, saved and reviewed plans, least-privilege permissions, audit logs, and serialized production applies.

## 50. How do you safely upgrade Terraform providers or modules?

Review release notes, update version constraints intentionally, regenerate and commit the lock file, test in an isolated environment, review the plan for replacements, back up state, and promote the change through environments.

---

<!-- Next section: Linux Interview Questions and Answers -->