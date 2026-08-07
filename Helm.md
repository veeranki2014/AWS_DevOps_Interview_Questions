```markdown
# Helm Interview Questions and Answers

## 1. What is Helm?

Helm is a package manager for Kubernetes. It packages Kubernetes manifests and related metadata into reusable units called charts and manages installed instances as releases.

## 2. What problem does Helm solve?

Helm reduces duplication in Kubernetes manifests, supports configurable deployments, packages related resources together, tracks release revisions, and provides upgrade and rollback operations.

## 3. What is a Helm chart?

A chart is a versioned package containing Kubernetes manifest templates, default configuration values, metadata, dependencies, and optional documentation or lifecycle hooks.

## 4. What is a Helm release?

A release is an installed instance of a chart combined with supplied configuration values. The same chart can be installed multiple times with different release names or values.

## 5. What is a Helm repository?

A Helm repository publishes packaged charts and an index describing available chart versions. OCI-compatible registries can also store and distribute Helm charts.

## 6. What is the standard structure of a Helm chart?

A typical chart contains:

```text
my-chart/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── service.yaml
│   └── NOTES.txt
└── README.md
```

## 7. What is `Chart.yaml`?

`Chart.yaml` contains chart metadata such as its name, description, chart version, application version, type, maintainers, and dependencies.

## 8. What is the difference between `version` and `appVersion`?

`version` identifies the chart package and should change when the chart changes. `appVersion` documents the application version but does not automatically determine the image tag or chart behavior.

## 9. What is `values.yaml`?

`values.yaml` contains the chart’s default configuration. Templates access these values through the `.Values` object.

## 10. How do you override Helm values?

Values may be overridden using additional files or command-line settings:

```bash
helm upgrade --install app ./chart \
  -f values-production.yaml \
  --set image.tag=1.2.3
```

Command-line overrides generally have higher precedence than supplied values files.

## 11. What are Helm templates?

Templates are Kubernetes YAML files containing Go-template expressions. Helm renders them using chart metadata, release information, capabilities, and supplied values.

## 12. What are common built-in Helm objects?

Common objects include:

- `.Values`
- `.Release`
- `.Chart`
- `.Capabilities`
- `.Template`
- `.Files`

## 13. What information does `.Release` provide?

`.Release` exposes release-specific information such as the release name, namespace, revision, whether the operation is an install or upgrade, and the release service.

## 14. What is `_helpers.tpl`?

`_helpers.tpl` conventionally stores reusable named templates, such as resource names, common labels, selectors, and service-account names.

## 15. How do you define and call a named template?

Define it using `define`:

```yaml
{{- define "app.labels" -}}
app.kubernetes.io/name: {{ .Chart.Name }}
{{- end }}
```

Call it using `include`:

```yaml
{{ include "app.labels" . | nindent 4 }}
```

## 16. Why is `include` often preferred over `template`?

`include` returns rendered text that can be passed through functions such as `indent`, `nindent`, `quote`, or `trim`. This makes YAML formatting easier to control.

## 17. What do `indent` and `nindent` do?

`indent` adds spaces to each line. `nindent` also inserts a leading newline before indenting, which is often useful when embedding generated YAML under a parent field.

## 18. How are conditional resources rendered?

Use `if`:

```yaml
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
...
{{- end }}
```

## 19. What does `with` do in a Helm template?

`with` executes a block when a value is non-empty and changes the current context to that value. Use `$` to access the root context from inside the block.

## 20. What does `range` do?

`range` iterates over a list, map, or other collection:

```yaml
{{- range .Values.hosts }}
- host: {{ . | quote }}
{{- end }}
```

## 21. How do you provide defaults in a template?

Use the `default` function:

```yaml
replicas: {{ .Values.replicaCount | default 1 }}
```

Defaults should normally be declared clearly in `values.yaml`; template defaults are useful for selected fallback behavior.

## 22. How do you require a value?

Use the `required` function:

```yaml
image: {{ required "image.repository is required" .Values.image.repository }}
```

Rendering fails with the specified message when the value is empty.

## 23. What is the `tpl` function?

`tpl` evaluates a string as a Helm template. It enables templated values but should be used carefully because it increases complexity and may evaluate user-controlled template expressions.

## 24. How do you render structured values as YAML?

Use `toYaml` with indentation:

```yaml
resources:
{{- toYaml .Values.resources | nindent 2 }}
```

## 25. Why is quoting important in Helm templates?

YAML type inference can convert strings into numbers or booleans. Quote string values when their type or formatting must be preserved.

## 26. What does `helm lint` do?

`helm lint` checks a chart for common structural and templating problems. It is useful in CI but does not replace rendering, schema validation, policy checks, or cluster-side validation.

## 27. What does `helm template` do?

It renders a chart locally without installing it:

```bash
helm template app ./chart -f values-test.yaml
```

The output can be inspected or passed to other validation tools.

## 28. What does `--dry-run` do?

A dry run renders and simulates an operation without completing a normal installation or upgrade. Depending on the command and options, server-side dry runs can also interact with the cluster API.

## 29. What is `values.schema.json`?

It is a JSON Schema describing supported chart values. Helm can use it to validate value types, required properties, patterns, ranges, and allowed choices.

## 30. What is the difference between `helm install` and `helm upgrade --install`?

`helm install` requires the release not to exist. `helm upgrade --install` upgrades an existing release or installs it if absent, making it convenient for idempotent deployment automation.

## 31. What does `helm upgrade` do?

It renders a new version using the selected chart and values, compares it with the current release, applies required Kubernetes changes, and records a new release revision.

## 32. How do you view Helm release history?

Use:

```bash
helm history RELEASE -n NAMESPACE
```

It displays revisions and their status.

## 33. How do you roll back a Helm release?

Use:

```bash
helm rollback RELEASE REVISION -n NAMESPACE
```

Rollback restores a prior Helm revision but may not reverse external side effects such as irreversible database changes.

## 34. What does `--atomic` do?

For an upgrade, `--atomic` attempts to roll back changes if the operation fails. It also enables waiting behavior. The rollback is limited to resources Helm manages and cannot undo every external side effect.

## 35. What do `--wait` and `--timeout` do?

`--wait` waits for supported resources to reach ready states before marking the operation successful. `--timeout` limits how long Helm waits for an individual operation.

## 36. What does `--cleanup-on-fail` do?

During an upgrade, it removes newly created resources if the upgrade fails. It does not provide a universal transaction across Kubernetes and external systems.

## 37. What are Helm hooks?

Hooks create resources at lifecycle points such as pre-install, post-install, pre-upgrade, post-upgrade, or test. They are commonly used for migrations, validation, or initialization tasks.

## 38. What are hook weights?

Hook weights control execution order. Hooks are sorted by weight, resource kind, and name; lower weights are processed first.

## 39. How are hook resources deleted?

Use hook deletion policies such as:

```yaml
helm.sh/hook-delete-policy: before-hook-creation,hook-succeeded
```

Without an appropriate policy or Job TTL, hook resources may remain in the cluster.

## 40. Why should hooks be used carefully?

Hooks have separate lifecycle behavior, can complicate upgrades and rollbacks, and may cause external side effects. Hook tasks should be idempotent, observable, and recoverable.

## 41. How are chart dependencies managed?

Declare dependencies in `Chart.yaml`, then run:

```bash
helm dependency update
```

This resolves dependencies and updates the lock file and local dependency packages.

## 42. What are library charts?

Library charts contain reusable template helpers but do not normally install Kubernetes resources directly. They help standardize behavior across application charts.

## 43. How does Helm handle CRDs?

CRDs placed in the chart’s `crds/` directory are installed before normal templates. Helm intentionally handles their upgrades and deletion conservatively, so teams need a deliberate CRD lifecycle strategy.

## 44. How should secrets be handled with Helm?

Do not store plaintext production secrets in chart values or Git. Use an external secret manager, an External Secrets operator, sealed-encrypted data, or another approved mechanism.

## 45. Can `--set` safely hide secrets?

No. Values may appear in shell history, process data, CI logs, and Helm release information. Use a secure secret-delivery process instead.

## 46. How do you support multiple environments?

Keep common defaults in the chart and use reviewed environment-specific values files. Avoid copying entire charts per environment, and keep sensitive data outside plaintext values files.

## 47. How do you design a reusable Helm chart?

Provide safe defaults, a documented values interface, schema validation, standard labels, configurable resources and probes, optional features, predictable naming, and minimal application-specific assumptions.

## 48. How should Helm charts be tested in CI?

Run linting, render each supported values combination, validate generated Kubernetes schemas, apply policy and security checks, test installation in an ephemeral cluster, and execute Helm tests where useful.

## 49. How do you debug a failed Helm deployment?

Inspect the release, rendered manifests, values, history, events, Pods, Jobs, hooks, API compatibility, and admission-policy failures:

```bash
helm status RELEASE -n NAMESPACE
helm get values RELEASE -n NAMESPACE
helm get manifest RELEASE -n NAMESPACE
kubectl get events -n NAMESPACE --sort-by=.metadata.creationTimestamp
```

## 50. Why might a Helm release be deployed but the application remain unhealthy?

Helm can successfully submit valid Kubernetes resources even when the application later fails. Check image availability, commands, configuration, secrets, probes, resources, scheduling, storage, networking, dependencies, and application logs.

