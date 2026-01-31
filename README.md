# Containerized Web Application Deployment with Docker

A Docker-based deployment workflow demonstrating how a web application can be containerized, versioned, distributed, and deployed with controlled networking and verifiable runtime behavior.

This project focuses on **container operations and validation**, not application development. The primary objective is to demonstrate practical Docker usage as it would occur in real-world DevOps environments.

---

## Project Context

Organizations delivering web-based solutions require deployments that are predictable, portable, and easy to reproduce across environments. Traditional server-based deployments often suffer from configuration drift, inconsistent dependencies, and manual setup steps.

This project demonstrates how Docker containers can be used to encapsulate a web application and its runtime dependencies into a single, portable artifact that can be deployed consistently while retaining flexibility in how it is exposed and networked at runtime.

---

## Deployment Overview

The deployment consists of a containerized Apache web server running inside an Ubuntu-based Docker container. The container is exposed externally via host-to-container port mapping and deployed on a user-defined Docker network to demonstrate explicit network control and isolation.

Host Machine
└── Docker Engine
├── Ubuntu Container
│ └── Apache Web Server (Port 80)
└── User-Defined Docker Network (app-net)

Host Port 8080 → Container Port 80 → Apache → HTML Content


---

## Implementation Flow

### Containerized Web Server Deployment

An Ubuntu container was launched in detached mode to serve as the application runtime. Apache was installed inside the container and configured to serve a custom static HTML page, representing a simple client-facing web service.

The container was exposed externally using Docker port mapping, allowing HTTP traffic from the host machine to reach the web server running inside the container.

Application availability was validated using:
- HTTP requests via `curl`
- Direct browser access

---

### Image Creation and Distribution

Once the container was verified to be functioning correctly, the running container was converted into a reusable Docker image. This image represents a versioned deployment artifact that can be distributed and redeployed consistently.

The image was published to Docker Hub, enabling:
- Artifact versioning
- Distribution across systems
- Re-deployment without manual reconfiguration

The published image was then pulled and deployed as a new container using a different host port, demonstrating that application behavior remains unchanged while access paths can vary by environment.

---

### Runtime Service Management

Because containers execute only the process defined at startup, the web server service was started explicitly inside the running container at runtime. This behavior was intentionally preserved to demonstrate an important operational characteristic of containers: services do not automatically start unless explicitly defined.

This mirrors real-world scenarios where service startup behavior must be managed intentionally through image configuration or runtime orchestration.

---

### Custom Docker Networking

To demonstrate network isolation and explicit network control, a user-defined Docker network was created. The containerized web application was deployed directly onto this network rather than relying on Docker’s default bridge network.

Network configuration was verified using Docker CLI inspection to confirm:
- The container was attached to the custom network
- The container was not attached to the default bridge network
- Application accessibility remained intact through host port mapping

---

## Validation & Evidence

All deployment steps were validated using Docker CLI tooling and runtime inspection rather than assumptions or GUI-only confirmation.

Validation included:
- Container and image inspection
- Port binding verification
- Process-level confirmation of the web server
- Network inspection from both container and network perspectives
- HTTP response validation via `curl`
- Browser-based access confirmation

Supporting evidence is captured in the `validation-screenshots/` directory.

---

## Key Observations

- Containers do not automatically start services unless defined at image build time or started explicitly at runtime
- Docker images capture both filesystem state and startup behavior
- Port remapping affects external access paths without modifying application configuration
- User-defined Docker networks provide clearer isolation and inspectability than the default bridge network
- CLI-based inspection is essential for validating container runtime behavior

---

## Notes on Image Construction

This project intentionally uses a runtime image creation approach to demonstrate container behavior and lifecycle mechanics. In production environments, this workflow would typically be replaced with a Dockerfile-based build defining a deterministic startup command.

A future iteration could extend this project to include:
- Dockerfile-based image builds
- Defined `CMD` or `ENTRYPOINT`
- Multi-container deployments using Docker Compose
- Integration into a CI/CD pipeline

---

## Conclusion

This project demonstrates practical Docker usage from container runtime through image distribution and network isolation. The emphasis on validation and inspection reflects real-world DevOps practices, where understanding system behavior is as critical as achieving a successful deployment.

The result is a portable, verifiable containerized web deployment suitable for repeatable use across environments.
