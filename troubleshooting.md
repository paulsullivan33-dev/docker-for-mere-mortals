# Troubleshooting

## The greatest hits

**"Unable to find image 'x' locally" then a pull error**

```text
Unable to find image 'hello-world:latest' locally
docker: Error response from daemon: ... failed to resolve reference ...
```

**Check (verified):** This exact shape appears when the image isn't
cached and the pull fails — usually a typo in the name (`ngnix`),
no network, or (behind a corporate proxy) the daemon not knowing
about the proxy. Verify the name on Docker Hub first; typos are the
#1 cause.

**"port is already allocated"**

```text
docker: Error response from daemon: driver failed programming external
connectivity: bind for 0.0.0.0:8080 failed: port is already allocated.
```

Something already uses host port 8080 — another container (`docker
ps` to find it) or a program on your machine. Use a different host
port (`-p 8081:80`).

**"permission denied" on /var/run/docker.sock (Linux)**

You're not in the `docker` group (see [Installing
Docker](installing-docker.md)). `sudo` works as a stopgap; the group
is the fix. Log out and back in after joining.

**Container exits immediately**

```bash
docker ps -a        # find it (it's stopped, so plain `ps` hides it)
docker logs <name>  # the answer is almost always here
```

The app crashed on startup — missing env var, wrong config, can't
reach the database. The logs say what; `docker run` without `-d` (so
you watch it fail live) says it louder.

**"Cannot connect to the Docker daemon"**

The engine isn't running. Linux: `sudo systemctl start docker`. Mac /
Windows: launch Docker Desktop and wait for the whale icon to stop
animating.

**Build is slow / re-downloads everything**

Something early in your Dockerfile changes every build, invalidating
the layer cache. Reorder: stable stuff (dependency installs) before
volatile stuff (your code). See [The Dockerfile](the-dockerfile.md).

**"no space left on device" during builds**

The build cache ate the disk. `docker system df` to confirm, `docker
system prune` (or `docker builder prune`) to reclaim. If it recurs,
raise the daemon's disk allocation (Docker Desktop settings) or prune
regularly.

## The method (same as ever)

1. `docker ps -a` — what exists, what state is it in?
2. `docker logs <name>` — what did it say before dying?
3. `docker inspect <name>` — what was it actually configured with?
4. Change one thing, retry.

And the Compose version: `docker compose ps`, `docker compose logs
<service>`. Same method, project scope.

**Next:** [Glossary](glossary.md)
