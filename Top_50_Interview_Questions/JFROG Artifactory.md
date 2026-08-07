```markdown
# JFrog Artifactory Interview Questions and Answers

## 1. What is JFrog Artifactory?

JFrog Artifactory is a universal artifact repository manager. It stores, organizes, secures, and distributes software packages and build artifacts throughout the software-delivery lifecycle.

## 2. What is an artifact repository?

An artifact repository stores versioned software outputs such as compiled binaries, libraries, container images, operating-system packages, archives, and deployment bundles.

Unlike a source-code repository, it primarily manages built and packaged software.

## 3. Why is Artifactory used in CI/CD?

Artifactory provides a central, controlled location where pipelines can publish and retrieve immutable build outputs. It improves reproducibility, traceability, access control, dependency management, and promotion.

## 4. Which package formats does Artifactory support?

Artifactory supports many formats, including:

- Maven and Gradle
- npm
- PyPI
- NuGet
- Docker and OCI
- Helm
- Debian
- RPM
- Go modules
- Conan
- Generic files

Support depends on the installed product version and configuration.

## 5. What is a local repository?

A local repository stores artifacts produced or uploaded by the organization. Examples include internal application packages, release binaries, and proprietary libraries.

## 6. What is a remote repository?

A remote repository acts as a caching proxy for an external repository such as Maven Central, npm, PyPI, or Docker Hub.

It reduces repeated downloads and provides a controlled dependency source.

## 7. What is a virtual repository?

A virtual repository presents multiple local and remote repositories through one URL. It simplifies client configuration and defines repository resolution order.

## 8. Compare local, remote, and virtual repositories.

- Local repositories contain organization-managed artifacts.
- Remote repositories proxy and cache external sources.
- Virtual repositories aggregate multiple repositories behind one endpoint.

## 9. Why should developers use a virtual repository URL?

A virtual repository provides a stable endpoint while administrators change backing repositories, ordering, security, or remote sources without requiring every client to be reconfigured.

## 10. How does remote-repository caching work?

When a client requests an artifact, Artifactory checks its cache. If the artifact is absent or requires metadata refresh, Artifactory retrieves it from the upstream source and stores a cached copy.

## 11. What happens when the upstream repository is unavailable?

Previously cached artifacts may remain available, depending on configuration and cache state. Artifacts that were never cached generally cannot be retrieved until the upstream source becomes available.

## 12. What is repository resolution order?

Resolution order determines which backing repository Artifactory checks first when a client requests an artifact through a virtual repository.

It should prioritize trusted internal releases and avoid ambiguous package resolution.

## 13. What is a generic repository?

A generic repository stores files that do not require a package-specific layout or metadata implementation. Examples include ZIP files, deployment bundles, configuration archives, and custom binaries.

## 14. What are snapshot and release repositories?

A snapshot repository stores changing development versions. A release repository stores finalized versions that should normally be immutable.

Separating them allows different retention, access, and immutability policies.

## 15. Why should release artifacts be immutable?

An immutable release version always represents identical content. This supports reproducible deployments, reliable rollback, auditing, and trustworthy promotion.

## 16. What is artifact promotion?

Promotion moves or copies an approved artifact through lifecycle stages such as development, testing, staging, and release without rebuilding it.

## 17. Why should an artifact be built only once?

Rebuilding per environment can produce different content because dependencies, timestamps, tools, or source state may change. Building once ensures production receives the artifact that was tested.

## 18. How should artifacts be versioned?

Use unique, traceable versions such as semantic versions, release numbers, or commit-derived identifiers. Avoid relying only on mutable labels such as `latest`.

## 19. What is build information?

Build information connects published artifacts with build metadata such as modules, dependencies, environment details, source revision, and pipeline identity.

It improves traceability between source, build, artifact, and deployment.

## 20. What is a build name and build number?

The build name identifies a pipeline or project. The build number identifies a particular execution or produced build within that name.

Together, they provide a stable reference to build information.

## 21. What is checksum-based storage?

Artifactory can store binaries according to their cryptographic checksums while repository paths reference that content. Identical binaries can therefore reuse stored content instead of creating unnecessary physical duplicates.

## 22. Which checksums are commonly associated with artifacts?

Common checksums include SHA-256, SHA-1, and MD5. Modern security validation should prefer stronger algorithms such as SHA-256.

## 23. How do you verify artifact integrity?

Retrieve the trusted checksum or signature through a protected channel and compare it with the downloaded artifact. In higher-assurance systems, use cryptographic signing and provenance verification.

## 24. How do clients authenticate to Artifactory?

Depending on configuration, clients may use access tokens, identity-provider integration, API credentials, or other supported authentication mechanisms.

Automation should use scoped, short-lived credentials where possible.

## 25. Why are access tokens preferable to long-lived passwords?

Tokens can be scoped, expired, rotated, and revoked independently. This reduces exposure and avoids sharing a human user’s permanent credentials with automation.

## 26. What are permission targets?

Permission targets associate users or groups with repositories, paths, and allowed operations such as reading, deploying, deleting, annotating, or managing content.

## 27. How would you implement least privilege?

Give developers read access to approved dependency repositories, grant CI identities deployment access only to required targets, restrict deletion and administration, and separate snapshot from release permissions.

## 28. How should CI/CD authenticate to Artifactory?

Use a dedicated machine identity or workload identity with a narrowly scoped token. Inject it securely at runtime and never store it in source code or pipeline logs.

## 29. How do you upload an artifact?

An artifact can be uploaded using a package client, JFrog CLI, REST API, or CI integration.

For generic files, a conceptual JFrog CLI command is:

```bash
jf rt upload "dist/*.zip" "generic-local/releases/"
```

## 30. How do you download an artifact?

Use the native package manager, JFrog CLI, or API:

```bash
jf rt download "generic-release/app/1.2.3/*" "download/"
```

Production automation should request an immutable version and verify its integrity.

## 31. What is JFrog CLI?

JFrog CLI is a command-line client for automating uploads, downloads, searches, builds, promotions, and other JFrog Platform operations.

## 32. What is AQL?

Artifactory Query Language, or AQL, searches Artifactory metadata using criteria such as repository, path, name, properties, size, creation time, and other fields.

## 33. What are artifact properties?

Properties are searchable key-value metadata associated with artifacts or folders. They can represent lifecycle state, ownership, environment, approval, retention category, or other organization-specific information.

## 34. How are properties different from artifact filenames?

Properties separate operational metadata from physical naming. An artifact can retain its immutable versioned path while its approval or lifecycle metadata changes.

## 35. What is an Artifactory cleanup policy?

A cleanup policy identifies artifacts eligible for deletion according to criteria such as age, download history, package type, repository, or release status.

Critical releases and legal-retention content should be explicitly protected.

## 36. Why is artifact cleanup necessary?

Without lifecycle management, storage usage, backup size, replication traffic, metadata volume, and operating cost continually grow.

Cleanup must preserve required releases, references, and audit evidence.

## 37. How would you safely implement retention?

Classify repositories and versions, protect promoted releases, simulate or report candidates first, review dependencies, retain backups, and automate deletion using narrow, tested rules.

## 38. What is garbage collection?

Garbage collection reclaims binary storage that is no longer referenced by repository metadata. Deleting a repository path does not always mean its physical binary data is immediately reclaimed.

## 39. What is high availability in Artifactory?

A high-availability deployment runs multiple application nodes against supported shared services and storage so traffic can continue if an individual application node fails.

It does not replace backups or disaster recovery.

## 40. What should be monitored in Artifactory?

Monitor:

- Service and node availability
- Request latency and error rates
- Storage capacity
- Database health
- Remote-repository failures
- Replication status
- Authentication failures
- JVM and system resources
- Backup success
- License and certificate expiry

## 41. How do you back up Artifactory?

Protect both metadata and binary storage using the supported backup approach for the deployment architecture. Record configuration, encryption keys, certificates, and external-service dependencies.

Recovery testing is essential.

## 42. What is the difference between high availability and disaster recovery?

High availability handles failures within the active environment. Disaster recovery restores service after a major site, region, storage, database, or configuration failure.

## 43. What is replication?

Replication synchronizes selected repository content and metadata between Artifactory instances. It can support distributed teams, availability, migration, and disaster-recovery designs.

## 44. Compare push and pull replication.

Push replication sends changes from a source instance to a destination. Pull replication causes the destination to retrieve content from a source.

The appropriate design depends on ownership, connectivity, and failure behavior.

## 45. How can Artifactory improve software supply-chain security?

It centralizes dependency sources, caches approved components, restricts publishing, records build metadata, integrates with scanning, supports signatures and provenance, and provides audit trails.

## 46. What is JFrog Xray?

JFrog Xray analyzes artifacts and dependencies for known vulnerabilities, license risks, and policy violations. It integrates with Artifactory and CI/CD workflows.

## 47. What should happen when a critical vulnerability is found?

Confirm applicability, identify affected builds and deployments, block unsafe promotion where appropriate, locate a patched dependency, rebuild the artifact, retest it, promote the replacement, and document any risk exception.

## 48. How do you troubleshoot an HTTP `401` or `403` response?

For `401`, verify credentials, token validity, authentication method, and time-related token issues.

For `403`, verify repository permissions, path scope, operation type, project membership, and policy restrictions.

## 49. How do you troubleshoot slow artifact downloads?

Check client connectivity, DNS, proxy behavior, Artifactory node load, storage latency, database health, remote-cache misses, upstream performance, replication, file size, and network throughput.

## 50. How would you design Artifactory for an enterprise?

Use separate repositories for lifecycle and trust boundaries, virtual repositories for clients, immutable releases, scoped identities, centralized dependency control, security scanning, retention policies, high availability, tested backups, disaster recovery, monitoring, and audited promotion.

