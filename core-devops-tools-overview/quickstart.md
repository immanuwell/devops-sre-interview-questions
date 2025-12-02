---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
metaLinks:
  alternates:
    - https://app.gitbook.com/s/yE16Xb3IemPxJWydtPOj/getting-started/quickstart
---

# Docker & container runtimes

### **Core Concepts**

* Docker packages applications into **containers**: isolated, reproducible environments.
* Containers share the **host OS kernel**, unlike VMs. Lightweight and fast.
* Containers run from **images** (immutable templates).
* A Dockerfile defines **how an image is built**.
* Layers are **cached**, enabling fast rebuilds.
* A container runtime (Docker, containerd, CRI-O) executes containers.

***

### **Docker Architecture**

* **Docker Engine** = Docker CLI + Docker API + Docker daemon + container runtime.
* Docker now uses **containerd** under the hood.
* **runc** is the low-level OCI-compliant runtime that actually spawns containers.
* Separation of concerns:
  * Docker CLI → user interface
  * Dockerd → builds & manages images/containers
  * containerd → container lifecycle
  * runc → executes containers according to OCI spec

***

### **Images & Layers**

* Docker images are **layered filesystems** (UnionFS).
* Each Dockerfile line creates a new **layer**.
* Layers are **read-only**; containers add a writable layer on top.
* Layers are **reused across images**, reducing disk usage.
* `COPY` and `RUN` create layers; `CMD` and `ENTRYPOINT` do not.

***

### **Container Lifecycle**

* Key states: **created → running → paused → stopped → removed**.
* `docker run` = create + start.
* Containers should be **stateless**; data must go to volumes.

***

### **Volumes & Storage**

* Volumes persist data outside container lifecycle.
* Types: **volumes**, **bind mounts**, **tmpfs**.
* Volumes recommended for production (managed by Docker).
* Bind mounts recommended for local development.

***

### **Networking**

* Default driver: **bridge**.
* Others: **host**, **none**, **overlay**, **macvlan**.
* Overlay networks commonly used in **Docker Swarm** and Kubernetes (via CNI).
* `docker run -p` maps container port → host port.

***

### **Dockerfile Best Practices**

* Use small base images (`alpine`, `distroless`, `scratch`).
* Use **multi-stage builds** to reduce size.
* Use `.dockerignore` to avoid huge contexts.
* Avoid `ADD` unless necessary; prefer `COPY`.
* Always define `USER` (drop root).
* Keep layers minimal and logical.

***

### **ENTRYPOINT vs CMD**

* **ENTRYPOINT**: fixed executable.
* **CMD**: default arguments.
* Combine both:

```dockerfile
ENTRYPOINT ["myapp"]
CMD ["--help"]
```

***

### **Security**

* Never run containers as **root**.
* Use **read-only root filesystem** when possible.
* Scan images with `docker scan` or Trivy.
* Use minimal images to reduce attack surface.
* Namespaces, cgroups, AppArmor/SELinux provide isolation.
* OCI runtimes enforce seccomp and capabilities.

***

### **containerd (Important for interviews)**

* containerd is a **container runtime** used by Docker and Kubernetes.
* Kubernetes uses containerd via **CRI** (Container Runtime Interface).
* Features:
  * Image management
  * Snapshotters (overlay2, btrfs, zfs)
  * Container lifecycle
  * gRPC API
* runc used by containerd for OCI container execution.
* Lightweight, fast, production-grade runtime.

***

### **OCI (Open Container Initiative)**

* OCI defines two specs:
  * **Image spec** (how images are structured)
  * **Runtime spec** (how containers are executed)
* Ensures compatibility between Docker, containerd, CRI-O, Podman.

***

### **Docker Compose**

* Compose manages multi-container applications.
* `docker-compose.yml` defines services, networks, and volumes.
* Used for development; not recommended for production.
* Can use profiles to enable/disable specific services.

***

### **Performance**

* Containers start in **milliseconds**.
* Low overhead since they share host kernel.
* Use `docker stats` for monitoring.
* Use build cache + multi-stage builds for optimal performance.

***

### **Common Interview Q\&A Insights**

* **Q: Docker vs containerd?** A: Docker is a full platform (CLI + daemon + build system); containerd is the low-level runtime Docker uses. Kubernetes uses containerd directly.
* **Q: How do containers isolate processes?** A: Linux **namespaces**, **cgroups**, **capabilities**, **seccomp**, and **rootfs**.
* **Q: Why are images immutable?** A: To ensure reproducibility, predictability, and fast deployment.
* **Q: How do you reduce image size?** A: Multi-stage builds, smaller base images, fewer layers, proper caching.
* **Q: Difference between `ENTRYPOINT` and `CMD`?** A: ENTRYPOINT = main command; CMD = default args.
* **Q: Persistent data strategy?** A: Volumes or external storage; containers should stay stateless.
