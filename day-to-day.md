# Day-to-Day Docker

The commands you'll actually type, in roughly the order you'll need
them.

## Seeing what's going on

```bash
docker ps              # running containers
docker ps -a           # including stopped ones
docker images          # images you have locally
docker volume ls       # volumes
docker stats           # live CPU/memory per container (q to quit)
```

## Looking inside

```bash
docker logs myweb              # what did it print?
docker logs -f myweb           # follow the logs live (Ctrl+C to stop)
docker logs --tail 50 myweb    # just the last 50 lines
docker exec -it myweb bash     # open a shell INSIDE the running container
docker inspect myweb           # everything Docker knows about it (JSON firehose)
```

`docker exec` is your debugging scalpel: the app misbehaves, you
`exec` in and look around — check files, try the network, read
*its* logs. (If the image has no shell, e.g. scratch-based, `exec`
can't help — check the image docs.)

## The lifecycle

```bash
docker stop myweb      # ask it to stop politely (SIGTERM, then SIGKILL after 10s)
docker start myweb      # start a stopped container (keeps its filesystem)
docker restart myweb    # stop + start (the classic fix)
docker rm myweb        # delete a stopped container
docker rm -f myweb     # delete a RUNNING container (stop + remove)
docker rmi myapp:1.0   # delete an image (only if no container uses it)
```

**stop vs. rm vs. rmi:** `stop` halts it (revivable with `start`);
`rm` deletes the container (its writable layer goes with it);
`rmi` deletes the *image* (the template). Volumes survive all three
unless you pass `-v` to `rm`.

## Cleaning up

Docker accumulates stopped containers, unused images, and build
cache. Reclaim it:

```bash
docker container prune   # delete ALL stopped containers (asks first)
docker image prune       # delete unused images
docker system prune     # containers, networks, build cache (asks first)
docker system df        # show what's using the space
```

**Careful:** `prune` is a bulldozer, not a scalpel. `docker system
prune -a` also deletes images not currently used by a container —
including ones you'll want next week. Read what it says it's about
to delete before confirming.

**Check:** `docker system df` shows reclaimable space per category —
run it before and after a prune to see what you won back.

## Updating a container to a new image version

Containers don't "upgrade" in place. The Docker way:

```bash
docker pull myapp:2.0
docker stop myapp && docker rm myapp
docker run -d --name myapp -v myappdata:/data myapp:2.0
```

Pull, replace, reattach the volume. (With Compose: bump the tag in
`compose.yaml`, then `docker compose up -d` — it recreates what
changed.)

**Next:** [Troubleshooting](troubleshooting.md)
