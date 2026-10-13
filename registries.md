# Registries

A **registry** is a server that stores images. **Docker Hub** is the
default public one — when you `docker pull nginx` with no registry
named, that's where it goes.

## Pulling and pushing

```bash
docker pull nginx:1.27              # download from Docker Hub
docker login                        # authenticate (once; stored locally)
docker tag myapp:1.0 paul/myapp:1.0  # rename for your namespace
docker push paul/myapp:1.0           # upload it
```

The image name determines the destination: `paul/myapp` pushes to
your Docker Hub account. Registries beyond Hub work the same way —
prefix the registry's address:

```bash
docker tag myapp:1.0 ghcr.io/paul/myapp:1.0
docker push ghcr.io/paul/myapp:1.0
```

(`ghcr.io` is GitHub's registry. AWS, Google, and private companies
run their own.)

## Official images and trust

Images named without a slash (`nginx`, `postgres`, `python`) are
**official images** — maintained by Docker with the upstream project.
They're the safest starting point. Everything else is some person's
(or company's) build:

- Check the publisher, the pull count, and when it was last updated.
  A database image untouched for three years is a vulnerability
  collection.
- Prefer the project's own namespace (e.g. `postgres` official)
  over random re-uploads.
- For critical stuff, pin by **digest** (the immutable SHA), not
  just tag: `nginx:1.27@sha256:abc123...`. Tags can be re-pointed;
  digests can't.

## Running your own registry (the 2-minute version)

```bash
docker run -d -p 5000:5000 --name registry registry:2
docker tag myapp:1.0 localhost:5000/myapp:1.0
docker push localhost:5000/myapp:1.0
```

The registry itself is just a container. Useful on a home network so
your machines share images without re-pulling from the internet.

## Rate limits (the honest footnote)

Docker Hub limits anonymous pulls (currently 100 per 6 hours per IP;
authenticated free accounts get more). If pulls suddenly fail with
"toomanyrequests," that's why — `docker login` (free account) raises
the ceiling. Limits have changed before; check Docker's docs for the
current numbers.

**Next:** [Day-to-Day Docker](day-to-day.md)
