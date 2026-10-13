# Images vs. Containers

This is the distinction the whole mental model hangs on. Get it once
and Docker stops feeling mysterious.

## The analogy: class vs. object (or recipe vs. cake)

- An **image** is the recipe: fixed, versioned, shareable. You can
  hand it to someone and they'll bake the identical cake.
- A **container** is the cake: a live instance, baked from the
  recipe. You can bake ten cakes from one recipe; eat one and the
  recipe is unaffected.

In Docker terms:

| Image | Container |
|-------|-----------|
| Read-only template | Running (or stopped) instance |
| Built once, shared many times | Created from an image, then disposable |
| Has a name and tag (`nginx:1.27`) | Has a name or ID (`myweb`, `a3f9c1...`) |
| `docker images` lists them | `docker ps` lists them |

## Tags are versions

```bash
docker pull nginx:1.27
docker pull nginx:latest
```

`nginx:1.27` means "the nginx image, version 1.27." The part after
the colon is the **tag**. `latest` is just a tag — it means "whatever
the publisher tagged latest," which usually (not always!) tracks the
newest release.

**The beginner trap:** `latest` doesn't mean "check for a newer
version." If you already have `nginx:latest` locally, `docker run
nginx` uses your cached copy without checking. For a fresh copy:
`docker pull nginx:latest` explicitly.

For anything you care about, pin the version (`nginx:1.27`, not
`nginx:latest`). Reproducibility is the point of Docker; `latest`
quietly undermines it.

## Layers: why pulls are fast (the second time)

An image is built in **layers** — each instruction in the Dockerfile
adds one. Layers are cached and shared: if ten images are all built
`FROM ubuntu:24.04`, that base layer is downloaded once and reused.

**Check:** Pull the same image twice:

```bash
docker pull hello-world
docker pull hello-world
```

The second run prints `Image is up to date` without downloading —
that's the layer cache (well, the tag check) at work.

## Looking at what you have

```bash
docker images
```

**Check (verified):** Columns are `REPOSITORY`, `TAG`, `IMAGE ID`,
`CREATED`, `SIZE`:

```text
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

(Empty until you've pulled or built something.)

```bash
docker ps        # running containers
docker ps -a     # ALL containers, including stopped ones
```

**Check (verified):** `docker ps -a` shows `CONTAINER ID`, `IMAGE`,
`COMMAND`, `CREATED`, `STATUS`, `PORTS`, `NAMES`. Beginners are
surprised by `-a`: without it, stopped containers are invisible, and
"where did my container go?" is always answered by `docker ps -a`.

## Containers are disposable — design for it

A container's writable layer dies with the container. `docker rm`
the container and anything it wrote inside is gone. That's not a bug;
it's the model. Data you want to keep goes in a **volume** —
[Volumes and Data](volumes-and-data.md).

**Next:** [The Dockerfile](the-dockerfile.md)
