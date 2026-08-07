```markdown
# Istio Interview Questions and Answers

## 1. What is Istio?

Istio is an open-source service mesh that provides traffic management, workload identity, service-to-service security, policy enforcement, and observability for distributed applications.

## 2. What is a service mesh?

A service mesh is an infrastructure layer that manages communication between application services. It moves capabilities such as mutual TLS, retries, routing, telemetry, and authorization out of individual application code.

## 3. Why would an organization introduce Istio?

Common reasons include:

- Service-to-service mutual TLS
- Consistent authorization
- Canary and traffic-splitting strategies
- Retries, timeouts, and circuit breaking
- Standardized metrics and traces
- Centralized traffic policy

Its operational cost should still be justified.

## 4. What are Istio’s control plane and data plane?

The control plane distributes configuration, identities, and service information. The data plane handles workload traffic and enforces the configuration.

## 5. What is Istiod?

Istiod is Istio’s main control-plane component. It provides service discovery, configuration distribution, certificate management, and conversion of Istio configuration into proxy configuration.

## 6. What is Envoy?

Envoy is a high-performance proxy commonly used in Istio’s sidecar data plane. It intercepts application traffic and implements routing, encryption, authorization, telemetry, retries, and resilience policies.

## 7. What is sidecar mode?

In sidecar mode, a proxy is deployed alongside each participating workload, normally in the same Pod. Application traffic passes through that proxy.

## 8. What is Istio ambient mesh?

Ambient mesh provides service-mesh capabilities without requiring a sidecar in every application Pod. It separates secure Layer 4 connectivity from optional Layer 7 processing through shared infrastructure.

## 9. Compare sidecar and ambient modes.

Sidecars provide mature per-workload Layer 7 processing but consume resources in every Pod. Ambient mode reduces per-Pod operational overhead while introducing node-level and shared waypoint components.

## 10. How is automatic sidecar injection enabled?

A namespace can be labeled for the desired Istio revision or injection behavior. Newly created Pods then receive an injected proxy through the admission process.

Existing Pods must be recreated.

## 11. How do you verify sidecar injection?

Check Pod containers and injection metadata:

```bash
kubectl get pod POD -o jsonpath='{.spec.containers[*].name}'
kubectl describe pod POD
```

A sidecar-mode Pod normally contains the application container and `istio-proxy`.

## 12. What is an Istio VirtualService?

A VirtualService defines how requests are routed to services. It can configure host matching, paths, headers, destinations, retries, timeouts, redirects, rewrites, fault injection, and traffic weights.

## 13. What is a DestinationRule?

A DestinationRule defines policies applied after traffic selects a destination. It commonly configures subsets, load balancing, connection pools, outlier detection, and TLS behavior.

## 14. How do VirtualServices and DestinationRules work together?

A VirtualService selects where traffic goes. A DestinationRule defines subsets and policies for those destinations.

For example, the VirtualService can send 10% of traffic to the `v2` subset defined by a DestinationRule.

## 15. What is an Istio subset?

A subset is a named group of service endpoints selected by labels:

```yaml
subsets:
  - name: v1
    labels:
      version: v1
```

The workload labels must match the subset definition.

## 16. How is a canary deployment implemented with Istio?

Deploy both versions, define subsets in a DestinationRule, and configure a VirtualService with weighted routes:

```yaml
route:
  - destination:
      host: app
      subset: v1
    weight: 90
  - destination:
      host: app
      subset: v2
    weight: 10
```

## 17. How can traffic be routed using headers?

A VirtualService can match a request header and select a destination. This supports internal testing, tenant-specific routing, or controlled preview releases.

## 18. What is an Istio Gateway?

A Gateway configures proxy listeners for traffic entering or leaving the mesh. It defines ports, protocols, hosts, and TLS settings but normally relies on a VirtualService for routing.

## 19. How does an Istio Gateway differ from Kubernetes Ingress?

Kubernetes Ingress is a general HTTP-routing API implemented by an Ingress controller. Istio Gateway resources configure Istio-managed gateway proxies and integrate with Istio’s traffic and security policies.

## 20. What is an ingress gateway?

An ingress gateway is an Istio-managed proxy deployment that receives traffic entering the mesh. It provides a controlled boundary for TLS, routing, telemetry, and policy.

## 21. What is an egress gateway?

An egress gateway provides a controlled exit point for selected outbound traffic. It can centralize monitoring, TLS origination, filtering, and network-policy enforcement.

## 22. What is a ServiceEntry?

A ServiceEntry adds an external or otherwise undiscovered service to Istio’s service registry. It lets the mesh apply routing, TLS, and policy to that destination.

## 23. What is mutual TLS?

Mutual TLS authenticates both sides of a connection and encrypts traffic in transit. In Istio, workload certificates are issued and rotated through the mesh identity system.

## 24. What is a PeerAuthentication policy?

PeerAuthentication controls the mutual-TLS mode accepted by workloads at the transport-authentication layer.

## 25. Compare `STRICT`, `PERMISSIVE`, and `DISABLE` modes.

- `STRICT` accepts only mutual-TLS traffic.
- `PERMISSIVE` accepts mutual TLS and plaintext.
- `DISABLE` disables mutual TLS for the selected scope.

`PERMISSIVE` is useful during migration but is weaker than an enforced strict configuration.

## 26. What is a RequestAuthentication policy?

RequestAuthentication validates request-level credentials, commonly JSON Web Tokens. It defines accepted issuers, audiences, and token locations.

## 27. What is an AuthorizationPolicy?

AuthorizationPolicy permits or denies traffic according to principals, namespaces, methods, paths, ports, request claims, and other attributes.

## 28. How would you allow only one service to call another?

Enforce mutual TLS, identify the caller through its workload identity, and apply an AuthorizationPolicy on the destination that permits only that principal and the required operations.

## 29. What is the difference between authentication and authorization?

Authentication establishes the caller’s identity. Authorization determines whether that identity may perform the requested action.

## 30. How do retries work in Istio?

A VirtualService can retry eligible failed requests a bounded number of times under selected conditions. Retries should use limited attempts and timeouts and are safest for idempotent operations.

## 31. Why can excessive retries be dangerous?

Retries increase load on an already failing dependency and can produce a retry storm. They also risk duplicating non-idempotent operations.

## 32. How are request timeouts configured?

A VirtualService can define an overall timeout for matching requests:

```yaml
timeout: 3s
```

Timeouts should align with application behavior and upstream and downstream latency budgets.

## 33. What is circuit breaking in Istio?

Circuit breaking limits connections, requests, pending requests, or retries to protect services from overload and cascading failure. It is configured primarily through DestinationRule connection-pool and outlier-detection settings.

## 34. What is outlier detection?

Outlier detection observes endpoint failures and temporarily ejects unhealthy endpoints from load balancing. It provides passive health management based on actual traffic.

## 35. What is fault injection?

Fault injection deliberately adds delays or failures to selected traffic. It is used to test application resilience without modifying application code.

## 36. What load-balancing policies does Istio support?

Depending on the environment and proxy support, policies can include round-robin, least-request, random, consistent-hash, and locality-aware behavior.

## 37. What is consistent-hash load balancing?

Consistent hashing selects endpoints using an attribute such as a header or cookie. It can support affinity but must be designed for endpoint changes and uneven traffic distributions.

## 38. What is locality-aware load balancing?

It prefers endpoints in the same region, zone, or subzone and can define failover behavior. It helps reduce latency and cross-zone traffic while maintaining resilience.

## 39. How does Istio provide observability?

Istio proxies emit standardized request metrics, access logs, and trace context. These can be collected by systems such as Prometheus, Grafana, OpenTelemetry, Jaeger, or other supported platforms.

## 40. Does Istio automatically provide complete distributed traces?

No. Applications generally must propagate trace headers across service calls, and a tracing backend and sampling configuration are required.

## 41. What is Kiali?

Kiali is an observability and management interface for Istio. It visualizes service topology, traffic, health, configuration, and selected metrics.

## 42. What overhead does a service mesh introduce?

Potential overhead includes proxy CPU and memory, additional latency, more connections, certificate operations, control-plane resources, configuration complexity, and extra troubleshooting layers.

## 43. How can sidecar resource consumption be controlled?

Set proxy CPU and memory requests and limits, tune concurrency and telemetry, disable unnecessary features, measure real traffic, and size nodes for both application and proxy resources.

## 44. Why might a service fail after sidecar injection?

Possible causes include port capture, health-probe behavior, strict mutual-TLS mismatches, authorization policies, unsupported protocols, startup ordering, excluded ports, resource pressure, or proxy configuration errors.

## 45. How do you troubleshoot an Istio `503` response?

Determine which proxy generated it, inspect response flags and access logs, verify endpoints, subsets, ports, TLS modes, policies, routing, readiness, DNS, and proxy configuration.

Useful commands include:

```bash
istioctl proxy-status
istioctl proxy-config routes POD
istioctl proxy-config clusters POD
istioctl analyze
```

## 46. What causes `no healthy upstream`?

The proxy has no eligible healthy destination endpoint. Causes include failed readiness, incorrect subset labels, missing endpoints, wrong ports, endpoint ejection, TLS failures, or restrictive policy.

## 47. What is `istioctl proxy-status` used for?

It compares connected proxies and their configuration synchronization with the control plane. It helps identify stale, disconnected, or partially synchronized proxies.

## 48. What does `istioctl analyze` do?

It statically analyzes Istio and Kubernetes configuration for common errors such as conflicting resources, missing references, ineffective selectors, and configuration inconsistencies.

## 49. How should Istio be upgraded safely?

Review supported upgrade paths, install a new control-plane revision, validate configuration, migrate selected workloads and gateways gradually, observe behavior, and remove the old revision only after successful migration.

## 50. When should Istio not be used?

Avoid introducing Istio when application size, security requirements, traffic complexity, or operational maturity do not justify it. Simpler Kubernetes networking, ingress, and observability components may be sufficient.

