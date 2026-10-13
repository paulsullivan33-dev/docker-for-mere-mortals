# Volumes and Data

A container's filesystem is disposable: delete the container, lose
what it wrote. For databases, uploads, or anything you care about,
store the data *outside* the container's writable layer. That's what
volumes are for.

## The rule

**Treat containers as cattle, data as pets.** The container can be
destroyed and recreated freely; the data lives on.

## Named volumes (the default choice)

```bash
docker volume create mydata
docker run -d --name db -v mydata:/var/lib/postgresql/data postgres:16
```

`-v mydata:/var/lib/postgresql/data` mounts the volume `mydata` at
that path inside the container. Postgres writes there thinking it's a
normal directory; Docker stores it in the volume. Delete the `db`
container and recreate it with the same `-v` — the data is still
there.

**Check (verified):**

```bash
docker volume ls
```

```text
DRIVER    VOLUME NAME
local     mydata
```

```bash
docker volume inspect mydata --format '{{.Mountpoint}}'
```

```text
/var/lib/docker/volumes/mydata/_data
```

That's where Docker keeps it on your machine. You rarely visit
directly — manage volumes through Docker commands:

```bash
docker volume ls        # list volumes
docker volume rm mydata  # delete one (only when no container uses it)
```

## Bind mounts (your files, live)

```bash
docker run -d --name web -v /home/paul/site:/usr/share/nginx/html nginx
```

A **bind mount** maps a directory *on your machine* into the
container. Edit files on your host, the container sees them
instantly. This is the development workflow: code on your machine,
run in the container.

**Careful:** Bind mounts punch a hole between host and container.
Mounting `/` or your home directory into an untrusted container hands
it your files. Mount the narrowest directory that works.

## Volumes vs. bind mounts

| | Named volume | Bind mount |
|---|---|---|
| Managed by | Docker | You (it's your path) |
| Best for | Databases, persistent app data | Development, config files |
| Survives container deletion | Yes | Yes (it's your disk) |
| Portable to another machine | No (stays on this host) | No |

## The anonymous-volume trap

```bash
docker run -v /data postgres:16   # no name before the colon!
```

That creates an *anonymous* volume — it works, but good luck finding
it later (`docker volume ls` shows a hash). Always name your volumes.

## Backing up a volume

Volumes live on the host, so back them up like anything else — but
stop the container first (or use the app's own backup) so you don't
copy a half-written database:

```bash
docker stop db
sudo tar -czf mydata-backup.tar.gz -C /var/lib/docker/volumes/mydata/_data .
docker start db
```

**Next:** [Ports and Networking](ports-and-networking.md)
