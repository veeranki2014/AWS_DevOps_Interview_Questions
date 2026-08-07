```markdown
# Azure DevOps Interview Questions and Answers

## 1. What is Azure DevOps?

Azure DevOps is Microsoft’s suite of software-delivery services. It includes Azure Boards, Repos, Pipelines, Test Plans, and Artifacts.

## 2. What are the primary Azure DevOps services?

The principal services are:

- Azure Boards for work tracking.
- Azure Repos for source control.
- Azure Pipelines for CI/CD.
- Azure Test Plans for testing.
- Azure Artifacts for package management.

## 3. What is an Azure DevOps organization?

An organization is the top-level container for projects, users, security settings, billing, agents, and shared resources within Azure DevOps.

## 4. What is an Azure DevOps project?

A project groups repositories, pipelines, boards, test plans, artifacts, teams, and permissions for a product or related collection of work.

## 5. What is Azure Repos?

Azure Repos provides hosted source control using Git repositories and, where required, Team Foundation Version Control.

## 6. What is Azure Boards?

Azure Boards provides work-item tracking, backlogs, Kanban boards, sprint planning, queries, dashboards, and traceability between requirements and development work.

## 7. What is Azure Pipelines?

Azure Pipelines is a CI/CD service that builds, tests, packages, and deploys applications on Microsoft-hosted or self-hosted agents.

## 8. What is the difference between YAML and classic pipelines?

YAML pipelines store pipeline definitions as code in version control. Classic pipelines are configured primarily through the user interface. YAML generally provides better review, history, reuse, and portability.

## 9. What is a pipeline trigger?

A trigger starts a pipeline in response to an event. Examples include repository changes, pull requests, schedules, completion of another pipeline, or manual execution.

## 10. How do you configure a CI trigger?

For example:

```yaml
trigger:
  branches:
    include:
      - main
  paths:
    include:
      - src/*
```

This triggers the pipeline for qualifying changes to `main`.

## 11. What is a pull-request trigger?

A pull-request trigger validates proposed changes before merging. The exact trigger behavior depends on repository type and the branch-policy or YAML configuration used.

## 12. What is a scheduled trigger?

A scheduled trigger runs a pipeline according to a cron-based schedule:

```yaml
schedules:
  - cron: "0 2 * * *"
    displayName: Nightly build
    branches:
      include:
        - main
```

Azure Pipelines cron schedules use UTC unless the surrounding configuration states otherwise.

## 13. What are stages, jobs, steps, and tasks?

- A stage is a major pipeline boundary such as build or production.
- A job is a collection of steps executed by one agent or execution target.
- A step is an individual pipeline operation.
- A task is a reusable packaged implementation of a step.

## 14. How do stages and jobs run by default?

Stages and jobs follow their dependency graph. Independent jobs may run in parallel when agents and parallel-job capacity are available.

## 15. How do you create dependencies?

Use `dependsOn`:

```yaml
stages:
  - stage: Build

  - stage: Deploy
    dependsOn: Build
```

Conditions can further control whether the dependent stage runs.

## 16. What are pipeline conditions?

Conditions determine whether a stage, job, or step executes:

```yaml
condition: succeeded()
```

Other common functions include `failed()`, `always()`, `succeededOrFailed()`, `and()`, and `eq()`.

## 17. What is an Azure Pipelines agent?

An agent is a machine that executes pipeline jobs. It downloads assigned work, runs tasks and scripts, and reports logs and results.

## 18. Compare Microsoft-hosted and self-hosted agents.

Microsoft-hosted agents are ephemeral and managed by Microsoft. Self-hosted agents offer custom software and private-network access but require security, updates, isolation, monitoring, and scaling.

## 19. What is an agent pool?

An agent pool groups agents that can execute pipeline jobs. Permissions and demands can control which pipelines and workloads use the pool.

## 20. What are agent demands and capabilities?

Capabilities describe properties available on an agent. Demands require matching capabilities before a job can be assigned to that agent.

## 21. What are variables in Azure Pipelines?

Variables are named values available to pipelines. They may be defined in YAML, variable groups, the pipeline interface, templates, scripts, or runtime expressions.

## 22. What is the difference between compile-time and runtime expressions?

Compile-time expressions use `${{ }}` while expanding templates and building the pipeline structure. Runtime expressions use `$[ ]` during execution and can access runtime information.

## 23. What is macro variable syntax?

Macro syntax uses:

```yaml
$(variableName)
```

It is commonly expanded before a task executes.

## 24. How do parameters differ from variables?

Parameters are typed and generally resolved during template expansion. Variables are primarily strings and can participate in runtime behavior.

## 25. How do you set a variable from a script?

A script can emit a logging command:

```bash
echo "##vso[task.setvariable variable=version]1.2.3"
```

Subsequent steps in the same job can use the value.

## 26. How do you pass an output variable between jobs?

Mark the variable as an output, give the producing step a name, and reference it through the dependency output syntax in the consuming job.

## 27. What are variable groups?

Variable groups centrally store values used by multiple pipelines. They can also be linked to an approved external secret source such as Azure Key Vault.

## 28. How should pipeline secrets be protected?

Use secret variables, protected variable groups, Key Vault integration, restricted permissions, and masked logs. Prefer workload identity federation over stored cloud credentials.

## 29. What is a service connection?

A service connection stores or brokers authentication and endpoint information for an external service such as Azure, Kubernetes, GitHub, Docker Registry, or another cloud.

## 30. How should Azure service connections authenticate?

Prefer workload identity federation or another short-lived identity mechanism. Scope access to the required subscription, resource group, environment, or resources.

## 31. What is a pipeline artifact?

A pipeline artifact is a file or package produced by one job or stage and published for later consumption, retention, or download.

## 32. Why should the same artifact be promoted through environments?

Building once and promoting the same immutable artifact ensures that production receives exactly what was tested. Rebuilding per environment can produce different content.

## 33. What is pipeline caching?

Caching restores reusable files such as dependency downloads between runs. It improves performance but should not replace durable artifact storage.

## 34. What are YAML templates?

Templates contain reusable stages, jobs, steps, or variable definitions. They help standardize pipelines and reduce duplication across repositories.

## 35. What is the difference between `include` and `extends` templates?

Include-style templates insert reusable content at a selected location. An `extends` template defines the governing pipeline structure that a consuming pipeline specializes through approved parameters.

## 36. How can templates enforce organizational standards?

Place approved security, build, and deployment logic in protected repositories. Use template restrictions, typed parameters, required checks, and controlled service connections.

## 37. What are Azure DevOps environments?

Environments represent deployment targets such as development, staging, production, Kubernetes namespaces, or virtual machines. They provide deployment history, security, and approval/check integration.

## 38. What are approvals and checks?

Approvals and checks control whether a protected resource can be used. They can require manual approval, business hours, external validation, branch restrictions, or other conditions.

## 39. What is an exclusive lock check?

An exclusive lock prevents conflicting pipeline stages from using a protected resource simultaneously. It is useful for serializing production deployments.

## 40. How do branch policies protect a repository?

Branch policies can require pull requests, minimum reviewers, linked work items, comment resolution, successful builds, selected merge strategies, and restrictions on direct pushes.

## 41. How do you implement manual production approval?

Create a production environment, configure its approval checks, and reference the environment from a deployment job. The stage pauses before accessing the environment.

## 42. What is a deployment job?

A deployment job is a specialized job that targets an environment and records deployment history. It supports deployment strategies such as `runOnce`, rolling, and canary where applicable.

## 43. How do you implement rollback?

Deploy immutable versioned artifacts, retain the last known-good release, validate deployment health, and provide a pipeline stage or operation that redeploys the previous version.

## 44. How do you integrate test results?

Run tests using a script or task and publish results using the relevant test-results task. The pipeline can then display failures, trends, attachments, and code-coverage information.

## 45. How do you integrate security scanning?

Add source, dependency, secret, IaC, container, and dynamic scanning at appropriate stages. Publish results, define risk-based quality gates, and provide controlled exception handling.

## 46. How do you troubleshoot a queued pipeline that does not start?

Check parallel-job capacity, matching agents, pool permissions, agent status, capabilities and demands, environment checks, resource authorization, and organization-level service health.

## 47. How do you troubleshoot a pipeline that passes locally but fails on an agent?

Compare operating systems, shells, tool versions, environment variables, permissions, path casing, credentials, network access, clean-workspace behavior, and resource limits.

## 48. How do you optimize a slow pipeline?

Measure stage and task duration, cache dependencies, parallelize independent work, reuse artifacts, use incremental builds, avoid redundant checkouts, right-size agents, and reduce unnecessary triggers.

## 49. How would you design a secure production pipeline?

Use protected branches, reviewed YAML, immutable artifacts, least-privilege federated identities, approved templates, security scans, protected environments, audit logs, deployment verification, and tested rollback.

## 50. How would you design a multi-environment pipeline?

Build and scan once, publish an immutable artifact, deploy it to development, run integration tests, promote it through protected staging and production environments, and retain deployment evidence and rollback information.

