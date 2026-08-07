```markdown
# Docker Interview Questions and Answers

## 1. What is Docker?

Docker is a platform for building, packaging, distributing, and running applications in containers. A container bundles an application with its required runtime, libraries, and configuration.

## 2. What problem do containers solve?

Containers provide consistent application environments across development, testing, and production. They improve portability, deployment speed, dependency isolation, and infrastructure utilization.

## 3. How are containers different from virtual machines?

Virtual machines emulate hardware and run separate guest operating systems. Containers share the host kernel while isolating processes, filesystems, networking, and resources, making them generally smaller and faster to start.

## 4. What is a Docker image?

A Docker image is an immutable, layered package containing an application, runtime, libraries, filesystem content, and metadata needed to create containers.

## 5. What is a Docker container?

A container is a runtime instance of an image. It adds a writable container layer and executes one or more processes in an isolated environment.

## 6. What is a Dockerfile?

A Dockerfile is a text file containing instructions for building an image:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

## 7. What happens when `docker run` is executed?

Docker locates or pulls the image, creates a writable container layer, configures isolation and networking, applies mounts and resource settings, and starts the configured process.

## 8. What is the difference between `docker create`, `start`, and `run`?

`docker create` creates a container without starting it. `docker start` starts an existing container. `docker run` creates and starts a new container.

## 9. What is the difference between `CMD` and `ENTRYPOINT`?

`ENTRYPOINT` defines the primary executable. `CMD` provides default arguments or a default command. Runtime arguments commonly replace `CMD` while remaining arguments to an exec-form `ENTRYPOINT`.

## 10. Why is exec form preferred for `CMD` and `ENTRYPOINT`?

Exec form starts the executable directly without an intermediate shell:

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

This generally provides better signal handling and predictable argument processing.

## 11. What is the difference between `COPY` and `ADD`?

`COPY` copies local build-context content into the image. `ADD` provides extra behavior, including automatic extraction of supported local archives and remote URL handling. Prefer `COPY` unless the additional behavior is specifically required.

## 12. What does the `FROM` instruction do?

`FROM` selects the base image for a build stage. Multiple `FROM` instructions create multiple stages in a multi-stage build.

## 13. What does `WORKDIR` do?

`WORKDIR` sets the working directory for following Dockerfile instructions and for the container’s default process. Docker creates the directory when necessary.

## 14. What is the difference between `ARG` and `ENV`?

`ARG` defines build-time variables. `ENV` sets environment variables that persist in the image and are available to containers unless overridden.

Neither should be used to embed secrets.

## 15. What is a multi-stage build?

A multi-stage build uses multiple `FROM` stages. Build tools and source files remain in an earlier stage while only required runtime artifacts are copied into the final image.

## 16. Why use multi-stage builds?

They reduce final image size and attack surface by excluding compilers, package managers, source files, caches, credentials, and other build-only dependencies.

## 17. How do Docker image layers work?

Most image-building instructions create immutable filesystem layers. Images reuse unchanged layers, reducing build time, storage, and registry transfer.

## 18. How does Docker build caching work?

Docker can reuse the cached result of a build step when the instruction and relevant inputs have not changed. Once a dependency changes, that step and affected later steps are rebuilt.

## 19. How do you optimize Dockerfile caching?

Copy dependency manifests first, install dependencies, and copy frequently changing application files afterward:

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

This avoids reinstalling dependencies for every source-code change.

## 20. What is `.dockerignore`?

`.dockerignore` excludes files from the build context. It reduces transfer time, cache invalidation, image bloat, and the risk of accidentally including secrets or local artifacts.

## 21. How do you reduce Docker image size?

Use a suitable minimal base, multi-stage builds, `.dockerignore`, runtime-only dependencies, combined package cleanup, and removal of temporary files and caches.

## 22. Why should containers run as a non-root user?

If an application is compromised, a non-root identity limits its privileges inside the container and reduces the impact of certain container-escape or mounted-resource attacks.

Example:

```dockerfile
USER 10001
```

## 23. What are Linux namespaces?

Namespaces isolate system resources such as process IDs, networking, mounts, hostnames, users, and interprocess communication between containers.

## 24. What are control groups?

Control groups, or cgroups, account for and limit resources such as CPU, memory, and process counts. They help prevent one container from consuming all host resources.

## 25. What is a Docker volume?

A volume is Docker-managed persistent storage outside a container’s writable layer. It can survive container deletion and be attached to replacement containers.

## 26. What is a bind mount?

A bind mount maps a specific host path into a container. It provides direct host-file access but creates tighter coupling to the host’s directory structure and permissions.

## 27. Compare volumes, bind mounts, and `tmpfs` mounts.

- Volumes are Docker-managed persistent storage.
- Bind mounts expose a host path.
- `tmpfs` stores temporary data in host memory and does not persist after removal or restart of the relevant container environment.

## 28. Why should important data not be stored in the writable container layer?

The writable layer is tied to the container lifecycle and is inefficient for many persistent workloads. Use an external database, object store, or mounted persistent storage instead.

## 29. What Docker network drivers are commonly used?

Common drivers include:

- `bridge` for local container networking.
- `host` for sharing the host network stack where supported.
- `none` for no external network interface.
- `overlay` for multi-host Docker Swarm networking.
- `macvlan` for assigning network-level identities to containers.

## 30. How do containers communicate on a user-defined bridge network?

Containers use assigned IP addresses and Docker’s embedded DNS. They can normally resolve each other using container or service names on the same network.

## 31. What does publishing a port do?

Port publishing maps a host address and port to a container port:

```bash
docker run -p 8080:80 nginx
```

Traffic received on host port `8080` is forwarded to container port `80`.

## 32. What is the difference between `EXPOSE` and `-p`?

`EXPOSE` documents the intended container port in image metadata. It does not publish the port. The `-p` or `--publish` option creates the host-to-container mapping.

## 33. Why might an application be reachable inside a container but not from the host?

Possible causes include missing port publication, binding only to `127.0.0.1` inside the container, incorrect ports, host firewall rules, network configuration, or an application startup failure.

## 34. Why does a container exit immediately?

A container normally runs only while its main PID is running. It exits when the foreground process completes, crashes, cannot start, or is terminated.

## 35. Why should a containerized service run in the foreground?

The main foreground process becomes the container’s PID 1 and determines its lifecycle. Running only a daemonized background process may cause the container to exit.

## 36. How do you inspect container logs?

Use:

```bash
docker logs container-name
docker logs --follow --tail 100 container-name
```

Production applications should emit structured logs to standard output and error for collection by the logging platform.

## 37. How do you inspect a running container?

Useful commands include:

```bash
docker inspect container-name
docker top container-name
docker stats container-name
docker exec -it container-name sh
```

Minimal or hardened images might not include a shell.

## 38. What is a Docker health check?

A health check periodically tests whether the application is functioning:

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:8080/health || exit 1
```

Health status does not automatically repair every container unless an orchestrator or other automation acts on it.

## 39. What are Docker restart policies?

Restart policies control whether Docker restarts a stopped container. Examples include `no`, `on-failure`, `always`, and `unless-stopped`.

## 40. How do you limit container resources?

Configure CPU, memory, process, and other limits:

```bash
docker run --memory 512m --cpus 1.0 --pids-limit 200 image
```

Applications should be tested to behave correctly under those constraints.

## 41. What happens when a container exceeds its memory limit?

The kernel may terminate processes in the container due to an out-of-memory condition. Docker reports relevant state information, often including an OOM-killed indicator.

## 42. How should secrets be supplied to containers?

Use an orchestrator’s secret mechanism or an external secret manager, preferably through short-lived identity or mounted files. Do not store secrets in Dockerfiles, image layers, repository files, or image environment defaults.

## 43. Why is deleting a secret in a later Dockerfile layer insufficient?

The secret may still exist in an earlier immutable image layer or build history. Prevent it from entering layers by using secure build-secret mounting or external retrieval.

## 44. What is an image registry?

A registry stores and distributes container images. Examples include Docker Hub, Amazon ECR, Azure Container Registry, GitHub Container Registry, and JFrog Artifactory.

## 45. How should images be tagged?

Use immutable identifiers such as commit-based tags and retain meaningful release tags where needed. Production deployments should ideally reference a digest for exact reproducibility.

## 46. What is an image digest?

A digest is a content-addressable cryptographic identifier, such as `sha256:...`. Unlike a mutable tag, it identifies specific image content.

## 47. How do you scan Docker images?

Use an image-scanning tool in CI and the registry to detect known vulnerabilities, secrets, malware, and policy violations. Establish severity policies and rebuild images when patched dependencies become available.

## 48. What is Docker Compose?

Docker Compose defines and runs multi-container applications using a YAML configuration. It describes services, networks, volumes, health checks, dependencies, and runtime configuration.

## 49. How does Docker Compose differ from Kubernetes?

Compose is primarily intended for relatively simple multi-container environments, especially local development. Kubernetes provides production-oriented orchestration, scheduling, self-healing, scaling, service discovery, policy, and extensibility.

## 50. How would you troubleshoot a failing container?

Check container state, exit code, logs, health status, command and entrypoint, configuration, secrets, mounts, permissions, network bindings, dependencies, architecture compatibility, and CPU or memory limits.

Useful commands include:

```bash
docker ps -a
docker logs container-name
docker inspect container-name
docker stats container-name
docker events
```

K