# Docker for Mere Mortals

"It works on my machine" is the most expensive sentence in software.
Docker fixes it by shipping the machine *with* the software: your app
plus everything it needs, packed into a **container** that runs the
same everywhere.

No CS degree required. Ideas first, then commands you can type.

## What this guide covers

- **What Docker is** — containers vs. virtual machines, and why the
  distinction matters
- **Installing Docker** — on Linux, Mac, and Windows
- **Your first container** — `docker run`, dissected piece by piece
- **Images vs. containers** — the single distinction that unlocks
  everything
- **The Dockerfile** — writing your own image, layer by layer
- **Volumes and data** — keeping data alive after the container dies
- **Ports and networking** — letting the world (or just you) in
- **Docker Compose** — multi-container apps in one file
- **Registries** — where images live (Docker Hub and friends)
- **Day-to-day Docker** — the commands you'll actually use
- **Troubleshooting** — the errors everyone hits

## What it doesn't cover

Kubernetes (that's orchestration — running containers across many
machines — and it's a guide of its own), Swarm, building production
pipelines, or Docker's internals. This guide is about understanding
and *using* Docker on your own machines.

## How to use this guide

Commands are meant to be typed. **Check:** blocks show what success
looks like. **Careful:** marks anything destructive.

## A note on verification

Image builds, volume and network management, and Compose file
validation were verified against Docker 29.1.3 and Compose v5.6.0.
Container *execution* examples (run output, logs, exec sessions) show
typical output — the verification sandbox blocks starting containers,
so confirm those on your own machine. Anything shown as typical
rather than measured is labeled.

## Chapters

1. [What Docker Is](what-docker-is.md)
2. [Installing Docker](installing-docker.md)
3. [Your First Container](your-first-container.md)
4. [Images vs. Containers](images-vs-containers.md)
5. [The Dockerfile](the-dockerfile.md)
6. [Volumes and Data](volumes-and-data.md)
7. [Ports and Networking](ports-and-networking.md)
8. [Docker Compose](docker-compose.md)
9. [Registries](registries.md)
10. [Day-to-Day Docker](day-to-day.md)
11. [Troubleshooting](troubleshooting.md)
12. [Glossary](glossary.md)

*Written to the [nine-point writing standard](criteria.md) shared by
every guide in this series.*

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Docker Compose for Mere Mortals](https://paulsullivan33-dev.github.io/docker-compose-for-mere-mortals/)
- [Podman for Mere Mortals](https://paulsullivan33-dev.github.io/podman-for-mere-mortals/)
- [Kubernetes for Mere Mortals](https://paulsullivan33-dev.github.io/kubernetes-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/docker-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/docker-for-mere-mortals/blob/main/LICENSE-CODE).
