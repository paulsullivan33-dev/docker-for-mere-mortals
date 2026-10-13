# Docker Compose

One container is a toy; real apps are several — a web app, a
database, maybe a cache. **Compose** describes the whole stack in one
YAML file and manages it as a unit.

## The file: compose.yaml

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - pgdata:/var/lib/postgresql/data

  web:
    build: .
    ports:
      - "8080:8000"
    depends_on:
      - db

volumes:
  pgdata:
```

Reading it: two **services**. `db` uses the published postgres image
with a password and a named volume. `web` builds from the Dockerfile
in this directory, publishes port 8000 as 8080, and starts after
`db`. The top-level `volumes:` declares `pgdata`.

Notes:

- `depends_on` controls *start order*, not readiness — `web` may
  start before postgres accepts connections. Apps should retry their
  DB connection (or use a wait-for script).
- `environment` here is fine for dev passwords; real secrets belong
  in a file referenced with `env_file`, not committed to git.

## The commands

```bash
docker compose up -d        # build (if needed), create, and start everything
docker compose ps           # status of this project's containers
docker compose logs -f      # follow all services' logs (Ctrl+C to stop)
docker compose down         # stop and remove containers and networks (keeps volumes!)
docker compose down -v      # ...and delete the volumes too (data loss — see Careful)
```

**Careful:** `down -v` deletes named volumes — your database. Plain
`down` keeps them, so `up` later finds the data intact. The `-v` is
for "I truly want a fresh start."

**Check (verified):** `docker compose config` renders and validates
the file without touching anything:

```bash
docker compose config --quiet && echo "valid"
```

```text
valid
```

It also shows the fully-resolved output (with the project name
prepended to networks) — handy for spotting typos.

## Why Compose instead of long docker run commands

- **One file, whole app.** `git clone` + `docker compose up -d` and a
  coworker runs your exact stack.
- **Names and networks handled.** Services reach each other by
  service name (`db`), on a network Compose creates.
- **Repeatable.** The file *is* the documentation of how the pieces
  fit — no wiki page of `docker run` incantations to go stale.

Compose is for one machine. When you outgrow that — many machines,
auto-scaling, self-healing — that's Kubernetes, a separate subject.

**Next:** [Registries](registries.md)
