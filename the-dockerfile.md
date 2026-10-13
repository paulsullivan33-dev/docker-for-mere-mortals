# The Dockerfile

A **Dockerfile** is a text recipe for building an image. Each
instruction adds a layer. Here's a complete, real one for a small
Python web app:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "app.py"]
```

Line by line:

| Instruction | What it does |
|-------------|--------------|
| `FROM python:3.12-slim` | Start from the official slim Python 3.12 image |
| `WORKDIR /app` | All following commands run in `/app` (created if needed) |
| `COPY requirements.txt .` | Copy the dependency list into the image |
| `RUN pip install ...` | Install dependencies — baked into the image |
| `COPY . .` | Copy the rest of your code in |
| `EXPOSE 8000` | Documentation: "this app listens on 8000" (doesn't publish it — see `-p`) |
| `CMD ["python", "app.py"]` | What runs when a container starts |

## Building it

```bash
docker build -t myapp:1.0 .
```

- `-t myapp:1.0` — **t**ag the result with a name and version.
- `.` — the build context: the directory sent to the Docker engine
  (the Dockerfile lives here by default).

**Check (verified):** A successful build ends with:

```text
Successfully built <id>
Successfully tagged myapp:1.0
```

Then `docker run -p 8000:8000 myapp:1.0` runs your app.

## The ordering trick (layer caching)

Docker caches each layer and reuses it if the instruction and its
inputs haven't changed. That's why `requirements.txt` is copied and
installed *before* the code: dependencies change rarely, code changes
constantly. Reorder it the other way and every code tweak reinstalls
all dependencies. Put the slow-changing stuff early, the
fast-changing stuff late.

## COPY vs. ADD, CMD vs. ENTRYPOINT

Two pairs that confuse everyone:

- **`COPY`** copies files. **`ADD`** copies files *plus* magic
  (auto-extracts tarballs, fetches URLs). Prefer `COPY` — explicit
  beats magic.
- **`CMD`** is the default command; anything after the image name in
  `docker run` *replaces* it. **`ENTRYPOINT`** is the fixed command;
  run arguments get *appended* to it. Most apps want `CMD`.

## .dockerignore

Like `.gitignore`, but for the build context:

```text
.git
__pycache__
*.pyc
.env
```

Without it, `COPY . .` drags your git history, caches, and secrets
into the image. Secrets especially: anything `COPY`'d into an image
is visible to anyone who gets the image. Never bake passwords or
keys into images — pass them at runtime with `-e` or files.

## Multi-stage builds (the 30-second version)

Build in one image, ship in another — so your final image doesn't
carry the compiler:

```dockerfile
FROM golang:1.23 AS build
WORKDIR /src
COPY . .
RUN go build -o /app/server .

FROM debian:12-slim
COPY --from=build /app/server /app/server
CMD ["/app/server"]
```

The final image has your compiled binary and nothing else. Smaller,
fewer vulnerabilities, faster to ship.

**Next:** [Volumes and Data](volumes-and-data.md)
