# Ports and Networking

Containers get their own private network. Nothing gets in or out
unless you say so — which is great for security and confusing the
first time your web app doesn't answer.

## Publishing ports: `-p`

```bash
docker run -d --name web -p 8080:80 nginx
```

`-p HOST:CONTAINER` — traffic to port 8080 *on your machine* goes to
port 80 *in the container*. The order is **host first, container
second**. Mnemonic: it reads like the path a request travels, from
the outside in.

**Check:** `http://localhost:8080` shows nginx. Without `-p`, the
container's port 80 exists but is unreachable from your machine —
the #1 "why isn't my app loading?" answer.

```bash
docker run -d -p 127.0.0.1:8080:80 nginx   # only reachable from this machine
docker run -d -P nginx                      # -P: publish all EXPOSEd ports on random host ports
```

Binding to `127.0.0.1` keeps a dev server off your LAN. Plain `-p
8080:80` listens on all interfaces — fine at home, think twice on a
public server.

## Container-to-container: use names

Containers on the same network reach each other **by container name**,
which Docker resolves like DNS:

```bash
docker network create appnet
docker run -d --name db --network appnet postgres:16
docker run -d --name api --network appnet myapp:1.0
```

Inside `api`, the database is at hostname `db`, port 5432. No IP
addresses to memorize, no `-p` needed between containers — `-p` is
only for traffic crossing into/out of the host.

**Check (verified):**

```bash
docker network ls
```

```text
NETWORK ID   NAME      DRIVER
...
```

(Docker creates a `bridge` network by default; user-created networks
like `appnet` get DNS between containers, the default bridge
doesn't — another reason to make your own.)

## The three network modes you'll meet

| Mode | What it does | When |
|------|--------------|------|
| `bridge` (default) | Private network, `-p` to expose | The normal case |
| `host` | Container shares the host's network directly | Max performance, no isolation |
| `none` | No networking at all | Locked-down batch jobs |

```bash
docker run --network host myapp:1.0   # skips port mapping entirely
```

## Debugging connectivity

Symptom → check:

- "Connection refused" on localhost → container isn't listening
  there, or you forgot `-p`. `docker ps` shows published ports under
  `PORTS`.
- Container can't reach another by name → are they on the same
  user-created network? (`docker network inspect appnet`)
- Worked yesterday, broken today → container got recreated with a
  new IP and something hardcoded the old one. Use names, not IPs.

**Next:** [Docker Compose](docker-compose.md)
