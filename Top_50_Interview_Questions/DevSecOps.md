```markdown
# DevSecOps Interview Questions and Answers

## 1. What is DevSecOps?

DevSecOps integrates security into software planning, development, testing, delivery, deployment, and operations. Security becomes a shared, automated, and measurable responsibility instead of a final release gate.

## 2. What does “shift left” mean?

Shift left means performing security activities earlier in development, such as threat modeling, secure coding, dependency scanning, and automated testing before production deployment.

## 3. What does “shift right” mean?

Shift right applies security controls and validation in running environments. Examples include runtime monitoring, attack simulation, incident response, configuration-drift detection, and production verification.

## 4. What are the core principles of DevSecOps?

Core principles include:

- Shared security ownership
- Automation
- Least privilege
- Secure defaults
- Continuous feedback
- Risk-based decisions
- Traceability
- Measurable improvement

## 5. How is DevSecOps different from traditional security?

Traditional security frequently reviews applications near release time. DevSecOps embeds security throughout delivery using developer enablement, automated controls, policy as code, and continuous monitoring.

## 6. What is a secure software development lifecycle?

A secure SDLC integrates security requirements, threat modeling, secure design, implementation controls, testing, release governance, monitoring, vulnerability management, and incident response into development.

## 7. What is threat modeling?

Threat modeling systematically identifies assets, trust boundaries, threat actors, attack paths, vulnerabilities, and mitigations before or during implementation.

## 8. What is STRIDE?

STRIDE is a threat-modeling framework covering:

- Spoofing
- Tampering
- Repudiation
- Information disclosure
- Denial of service
- Elevation of privilege

## 9. When should threat modeling be performed?

Perform it during architecture and design, then update it when trust boundaries, data flows, dependencies, authentication, authorization, or deployment architecture change.

## 10. What is defense in depth?

Defense in depth uses multiple independent security controls so failure of one control does not immediately compromise the entire system.

## 11. What is the principle of least privilege?

Least privilege grants identities only the permissions required for approved tasks, at the narrowest resource scope and for the shortest practical duration.

## 12. What is zero trust?

Zero trust assumes network location alone does not establish trust. Every request should be authenticated, authorized, encrypted where appropriate, evaluated using relevant context, and continuously monitored.

## 13. What is SAST?

Static Application Security Testing analyzes source code, bytecode, or binaries without running the application. It detects patterns such as unsafe input handling, injection risks, and insecure API usage.

## 14. What is DAST?

Dynamic Application Security Testing evaluates a running application from the outside. It sends requests and observes behavior to identify exploitable weaknesses.

## 15. What is IAST?

Interactive Application Security Testing instruments or observes a running application while tests execute. It combines runtime context with code-level insight.

## 16. What is SCA?

Software Composition Analysis identifies third-party and open-source dependencies, known vulnerabilities, licenses, and sometimes transitive dependency risks.

## 17. What is secret scanning?

Secret scanning detects exposed credentials, API keys, tokens, certificates, and other sensitive values in source code, history, build logs, images, and artifacts.

## 18. What is Infrastructure as Code scanning?

IaC scanning evaluates Terraform, Kubernetes, CloudFormation, Bicep, Helm, and similar configuration for insecure settings and policy violations before deployment.

## 19. What is container-image scanning?

Container scanning examines image packages, operating-system components, application dependencies, secrets, configuration, malware indicators, and metadata for known risks.

## 20. Can a successful vulnerability scan prove an application is secure?

No. Scanners have incomplete coverage and may produce false positives or negatives. Security also requires design review, testing, runtime protection, operational controls, and human judgment.

## 21. Where should security scans run in CI/CD?

Run fast checks during pull requests, deeper checks during builds, artifact and image scans before promotion, deployment-policy checks before release, and continuous scanning after publication.

## 22. Should every vulnerability fail a pipeline?

Not necessarily. Gates should consider exploitability, severity, asset criticality, environment, fix availability, exposure, compensating controls, and organizational risk policy.

## 23. What is a security quality gate?

A security quality gate evaluates results against an approved risk policy and prevents promotion when unacceptable findings are present.

It should support documented, expiring exceptions.

## 24. How should false positives be handled?

Validate the finding, record evidence, suppress it narrowly, assign an owner, set an expiration date, and periodically review whether the exception remains valid.

## 25. What is a software bill of materials?

An SBOM is an inventory of software components and dependency relationships contained in an application or artifact.

Common formats include SPDX and CycloneDX.

## 26. Why is an SBOM useful?

It helps organizations identify affected applications when a dependency vulnerability, license issue, or supply-chain incident is discovered.

## 27. What is artifact provenance?

Provenance records where, when, and how an artifact was built, including source identity, build system, inputs, and relevant process metadata.

## 28. What is artifact signing?

Artifact signing uses cryptography to associate an artifact with an approved signer or build process. Consumers verify the signature before trusting or deploying it.

## 29. What is SLSA?

Supply-chain Levels for Software Artifacts is a framework for improving build integrity and software-supply-chain security through progressively stronger controls and provenance.

## 30. What is policy as code?

Policy as code expresses security and compliance rules in machine-evaluable files. It enables version control, review, testing, and automated enforcement.

## 31. Where can policy as code be enforced?

It can be enforced during pull requests, CI builds, artifact promotion, cloud provisioning, Kubernetes admission, and continuous runtime compliance evaluation.

## 32. What is an admission controller?

A Kubernetes admission controller evaluates API requests after authentication and authorization but before object persistence. It can validate, reject, or mutate resources according to policy.

## 33. What Kubernetes security policies should commonly be enforced?

Examples include:

- Non-root execution
- Restricted privilege escalation
- Approved image registries
- Immutable image references
- Resource limits
- Safe capabilities
- Protected host namespaces
- Restricted host mounts
- Required security profiles

## 34. How should secrets be managed?

Use a dedicated secrets manager, short-lived credentials, workload identity, encryption, least-privilege access, rotation, audit logging, and secure runtime injection.

Never store plaintext production secrets in Git.

## 35. What should happen when a secret is committed to Git?

Revoke or rotate it immediately, investigate its use, remove unnecessary historical exposure, notify affected owners, add detection and prevention, and document the incident.

Deleting the current line does not invalidate an exposed credential.

## 36. Why are short-lived credentials safer?

They automatically expire and reduce the useful lifetime of stolen credentials. They also support contextual trust and avoid distributing permanent secrets.

## 37. What is workload identity federation?

Workload identity federation lets CI/CD or applications exchange a trusted identity assertion for temporary cloud credentials without storing long-lived cloud keys.

## 38. How should CI/CD systems be secured?

Use protected branches, reviewed pipeline definitions, isolated runners, minimal token permissions, short-lived credentials, pinned dependencies, protected environments, immutable artifacts, and comprehensive audit logs.

## 39. Why should third-party CI actions and plugins be pinned?

Mutable tags can be changed to reference different code. Pinning to an immutable version or commit reduces the chance of silently executing altered or compromised automation.

## 40. How should self-hosted runners be secured?

Use ephemeral isolated runners where possible, restrict network access and secrets, patch images, remove credentials after jobs, prevent untrusted workloads from reaching privileged runners, and monitor execution.

## 41. What is dependency confusion?

Dependency confusion occurs when a package manager retrieves a malicious public package instead of an intended internal package with the same name.

Use controlled registries, namespace ownership, source mapping, and strict resolution rules.

## 42. What is typosquatting?

Typosquatting publishes malicious packages with names similar to legitimate dependencies, hoping users or automation install them accidentally.

## 43. What is runtime application security?

Runtime security monitors and controls behavior after deployment. It may include workload detection, network controls, API protection, process monitoring, anomaly detection, and incident-response automation.

## 44. What is vulnerability management?

Vulnerability management continuously discovers, prioritizes, assigns, remediates, verifies, and reports vulnerabilities across code, dependencies, images, infrastructure, and running systems.

## 45. How should vulnerabilities be prioritized?

Consider exploitability, exposure, asset criticality, known exploitation, data sensitivity, privileges, reachable attack paths, compensating controls, and remediation availability—not CVSS alone.

## 46. What is mean time to remediate?

Mean time to remediate measures the average time between identifying a security issue and completing an accepted remediation.

It should be segmented by severity and system criticality.

## 47. What DevSecOps metrics are useful?

Useful metrics include:

- Vulnerability age
- Remediation SLA compliance
- Mean time to remediate
- Secret-exposure rate
- Scan coverage
- Exception count and age
- Signed-artifact coverage
- Patch latency
- Security-related deployment failure rate

Metrics should encourage risk reduction rather than superficial compliance.

## 48. How should security exceptions be managed?

Each exception should document the risk, business justification, affected scope, owner, compensating controls, approval, expiration date, and remediation plan.

Exceptions should expire automatically unless reviewed.

## 49. How would you respond to a critical production vulnerability?

Validate impact, identify affected assets, assess active exploitation, notify incident and service owners, apply containment, patch or mitigate, rebuild and redeploy trusted artifacts, verify remediation, and preserve evidence for review.

## 50. How would you design a DevSecOps pipeline?

A strong pipeline typically performs:

1. Pre-commit secret and quality checks.
2. Pull-request SAST, SCA, IaC, and policy scans.
3. Reproducible builds on isolated runners.
4. Unit and integration testing.
5. SBOM and provenance generation.
6. Artifact and container scanning.
7. Artifact signing and immutable publication.
8. Protected environment approval.
9. Deployment-policy enforcement.
10. Runtime monitoring and continuous vulnerability management.

---

# Complete Topic Index

1. Azure
2. AWS
3. Terraform
4. Linux
5. GitHub Actions
6. Docker
7. Kubernetes
8. Helm
9. Argo CD
10. Istio
11. Azure DevOps
12. Shell Scripting
13. Python
14. JFrog Artifactory
15. DevSecOps

---

## Suggested Repository Structure

```text
Top_50_Interview_Questions/
├── README.md
├── Azure.md
├── AWS.md
├── Terraform.md
├── Linux.md
├── GitHub_Actions.md
├── Docker.md
├── Kubernetes.md
├── Helm.md
├── Argo_CD.md
├── Istio.md
├── Azure_DevOps.md
├── Shell_Scripting.md
├── Python.md
├── Artifactory.md
└── DevSecOps.md
```
