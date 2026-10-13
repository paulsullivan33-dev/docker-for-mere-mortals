# Glossary

| Term | Meaning |
|------|---------|
| bind mount | Map a host directory into a container (live, two-way) |
| build context | The directory sent to the engine during `docker build` |
| Compose | Tool for defining multi-container apps in a YAML file |
| container | A running (or stopped) instance of an image — disposable by design |
| daemon / engine | The background service that builds, runs, and manages containers |
| digest | An image's immutable SHA identifier — pinning by digest can't be re-pointed |
| Dockerfile | Text recipe describing how to build an image |
| ENTRYPOINT | The fixed command a container runs (run args append to it) |
| CMD | The default command a container runs (run args replace it) |
| image | A read-only template for containers — the sealed package |
| layer | One step's worth of an image; layers are cached and shared |
| namespace | Kernel feature Docker uses for isolation (processes, network, mounts) |
| port publishing (`-p`) | Map a host port to a container port (`-p HOST:CONTAINER`) |
| registry | A server storing images (Docker Hub is the default public one) |
| tag | An image version label (`nginx:1.27`); `latest` is just a tag |
| volume | Docker-managed persistent storage, mounted into containers |
