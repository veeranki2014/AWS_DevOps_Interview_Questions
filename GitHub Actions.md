```markdown
# GitHub Actions Interview Questions and Answers

## 1. What is GitHub Actions?

GitHub Actions is GitHub’s automation platform. It can build, test, scan, package, release, and deploy code in response to repository events, schedules, or manual requests.

## 2. What is a GitHub Actions workflow?

A workflow is an automated process defined in a YAML file under:

```text
.github/workflows/
```

A workflow contains triggers, permissions, jobs, steps, environment settings, and other execution controls.

## 3. What is a workflow event?

An event is an activity that can trigger a workflow. Examples include `push`, `pull_request`, `workflow_dispatch`, `schedule`, `release`, and `workflow_call`.

## 4. What is a job?

A job is a collection of steps executed on the same runner. Jobs run in parallel by default unless dependencies or concurrency controls impose an order.

## 5. What is a step?

A step is an individual task within a job. It either runs a command or invokes an action.

## 6. What is an action?

An action is a reusable unit of automation. Actions can be implemented using JavaScript, Docker containers, or composite steps.

## 7. What is a runner?

A runner is the machine or execution environment that runs a job. GitHub provides hosted runners, while organizations can also register self-hosted runners.

## 8. Compare GitHub-hosted and self-hosted runners.

GitHub-hosted runners provide clean, managed environments and require little maintenance. Self-hosted runners provide greater control and private-network access but require patching, isolation, scaling, security, and lifecycle management.

## 9. What does `runs-on` do?

`runs-on` selects the runner environment for a job:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
```

It can also reference self-hosted runner labels or runner groups.

## 10. How do jobs run sequentially?

Use `needs` to create a dependency:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

  deploy:
    needs: build
    runs-on: ubuntu-latest
```

The `deploy` job starts only after `build` completes successfully unless its condition says otherwise.

## 11. How do you run a job even if a dependency fails?

Use a status-check function:

```yaml
if: ${{ always() }}
```

For example, a notification or cleanup job may need to run regardless of earlier results.

## 12. What are expressions and contexts?

Expressions use `${{ }}` to evaluate information from contexts such as `github`, `env`, `vars`, `secrets`, `needs`, `steps`, `runner`, and `matrix`.

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

## 13. How do you define environment variables?

Environment variables can be defined at workflow, job, or step level:

```yaml
env:
  APP_ENV: test
```

More specific scopes override broader ones with the same name.

## 14. How do repository variables differ from secrets?

Variables store non-sensitive configuration. Secrets are intended for sensitive values and receive masking and restricted handling, although workflows must still avoid exposing them.

## 15. How should secrets be used securely?

Store secrets in GitHub-managed secret scopes, grant minimal workflow permissions, avoid printing values, restrict environments, prefer short-lived identity federation, and do not pass secrets to untrusted code.

## 16. What is `GITHUB_TOKEN`?

`GITHUB_TOKEN` is a short-lived token created for a workflow job. It can authenticate to GitHub APIs and repository operations according to the permissions granted to that job.

## 17. How do you restrict `GITHUB_TOKEN` permissions?

Declare only the permissions required:

```yaml
permissions:
  contents: read
  packages: write
```

Permissions can be configured at workflow or job level.

## 18. Why should workflow permissions be explicitly declared?

Explicit permissions reduce the impact of a compromised dependency or workflow step. They also make the workflow’s required access easier to review.

## 19. What is OpenID Connect in GitHub Actions?

OIDC lets a workflow request a short-lived identity token that a cloud provider validates. It avoids storing long-lived cloud access keys in GitHub.

## 20. How would a workflow securely authenticate to AWS?

Configure an AWS IAM role that trusts GitHub’s OIDC provider and restricts allowed repository, branch, tag, or environment claims. The workflow requests temporary role credentials at runtime.

## 21. How would a workflow securely authenticate to Azure?

Configure a Microsoft Entra application or managed identity with a federated credential that trusts the intended GitHub repository and context. The workflow then exchanges its OIDC token for short-lived Azure access.

## 22. What is a matrix strategy?

A matrix creates multiple job variations from combinations of values:

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    python: ["3.11", "3.12"]
```

This example creates four job combinations.

## 23. What are `fail-fast` and `max-parallel`?

`fail-fast` controls whether remaining matrix jobs are cancelled after a failure. `max-parallel` limits how many matrix jobs execute simultaneously.

## 24. How do you pass data between steps?

Write the value to `$GITHUB_OUTPUT` in a step with an `id`:

```yaml
- id: version
  run: echo "value=1.2.3" >> "$GITHUB_OUTPUT"

- run: echo "${{ steps.version.outputs.value }}"
```

## 25. How do you pass data between jobs?

Expose a step output as a job output and reference it through the `needs` context:

```yaml
jobs:
  build:
    outputs:
      version: ${{ steps.meta.outputs.version }}
```

A dependent job can read `needs.build.outputs.version`.

## 26. What are workflow artifacts?

Artifacts are files uploaded from a workflow for later jobs or users. Examples include test reports, binaries, coverage reports, and deployment packages.

## 27. What is dependency caching?

Caching reuses files such as downloaded dependencies across workflow runs. It can reduce build time but should not be treated as permanent artifact storage.

## 28. How do artifacts and caches differ?

Artifacts preserve workflow outputs for retrieval or transfer. Caches optimize repeated work and may be evicted or replaced. A successful release should not depend exclusively on a cache.

## 29. What are reusable workflows?

Reusable workflows are complete workflows invoked using `workflow_call`. They can contain multiple jobs and provide standardized CI/CD processes across repositories.

## 30. What is a composite action?

A composite action packages multiple steps as one reusable action. It is useful for repeated step-level behavior but does not independently define multiple jobs.

## 31. When should you use a reusable workflow instead of a composite action?

Use a reusable workflow when standardizing complete jobs, permissions, runners, environments, or pipelines. Use a composite action when reusing a sequence of steps within a job.

## 32. What is `workflow_dispatch`?

`workflow_dispatch` enables manual workflow execution. It can define inputs such as target environment, application version, or deployment option.

## 33. What is `workflow_call`?

`workflow_call` makes a workflow callable by another workflow. It can define typed inputs, secrets, and outputs.

## 34. How do branch and path filters work?

Triggers can include or exclude selected branches, tags, and file paths:

```yaml
on:
  push:
    branches: [main]
    paths:
      - "src/**"
```

Both filters must be designed carefully so required checks are triggered consistently.

## 35. What are GitHub environments?

Environments represent deployment targets such as development, staging, or production. They can contain scoped secrets and variables and may enforce reviewers, branch rules, or other protection controls.

## 36. How do you require production approval?

Create a protected production environment, configure required reviewers, and assign the deployment job to it:

```yaml
environment: production
```

The job waits until the configured protection rules are satisfied.

## 37. What is a concurrency group?

Concurrency controls prevent or limit overlapping workflow runs or jobs that share a key:

```yaml
concurrency:
  group: production
  cancel-in-progress: false
```

This is useful for serializing deployments.

## 38. How do you cancel outdated workflow runs?

Use a concurrency group based on the workflow and branch and enable cancellation:

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

## 39. How do you execute cleanup steps after failure?

Use:

```yaml
if: ${{ always() }}
```

The cleanup logic should also tolerate partially created resources and missing files.

## 40. How can failed commands be retried?

GitHub Actions does not automatically make every step retryable. Implement bounded retry logic in a script or use a carefully reviewed retry action, and retry only genuinely transient operations.

## 41. How would you build and push a Docker image?

A typical workflow:

1. Checks out the repository.
2. Authenticates to the registry.
3. Generates immutable image tags.
4. Configures Buildx.
5. Builds and tests the image.
6. Pushes it only from an approved event.
7. Records its digest and provenance.

## 42. How should Docker images be tagged?

Use an immutable tag such as a commit SHA, optionally combined with a semantic-version tag. Deployments should ideally reference an image digest to prevent tag mutation from changing deployed content.

## 43. How do you protect workflows triggered from forks?

Do not expose privileged secrets to untrusted pull-request code. Use read-only permissions, avoid checking out untrusted code in privileged contexts, and carefully separate validation from trusted deployment workflows.

## 44. Why is `pull_request_target` security-sensitive?

It runs in the base repository’s security context and may have access to secrets or write permissions. Executing code from the pull request in that context can compromise the repository.

## 45. How should third-party actions be secured?

Use trusted actions, review their source and permissions, and pin them to immutable commit SHAs. Dependabot or another controlled process can propose reviewed updates.

## 46. How do you debug a failed workflow?

Inspect the failing job and step, enable additional diagnostic logging when appropriate, verify contexts and permissions, check runner differences, reproduce commands locally, and examine artifacts or service logs.

## 47. Why might a workflow work locally but fail on a runner?

Possible causes include different operating systems, shells, tool versions, file permissions, path casing, environment variables, credentials, network access, resource limits, or missing dependencies.

## 48. How do you design a rollback-capable deployment workflow?

Deploy immutable artifacts, record the previous version, verify health after deployment, stop promotion on failure, and provide an automated operation that redeploys the known-good version.

## 49. How would you design a production CI/CD workflow?

A strong design includes linting, unit and integration tests, dependency and security scans, reproducible builds, immutable artifacts, protected environments, short-lived credentials, deployment verification, auditability, and rollback.

## 50. How do you optimize a slow GitHub Actions workflow?

Measure job and step durations, run independent jobs in parallel, cache dependencies correctly, avoid repeated setup, use reusable artifacts, reduce unnecessary triggers, right-size runners, and remove redundant work without weakening validation.

---

<!-- Next section: Docker Interview Questions and Answers -->
```