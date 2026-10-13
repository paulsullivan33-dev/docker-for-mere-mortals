# Your First Container

```bash
docker run hello-world
```

That one command did four things. Let's dissect it, because `docker
run` is the command you'll use most, and its flags are worth
understanding rather than memorizing.

## What just happened

1. Docker looked for an image named `hello-world` locally. Didn't
   find it.
2. It downloaded ("pulled") it from Docker Hub, the public image
   registry.
3. It created a container from the image and started it.
4. The container printed its message and exited.

**Check (typical output, abbreviated):**

```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

The `Unable to find image locally` line isn't an error — it's Docker
telling you step 1 failed over to step 2.

## The flags that matter

```bash
docker run -it ubuntu bash
```

- `-i` — keep stdin open (so you can type)
- `-t` — give me a terminal (so it looks right)
- `-it` together — "I want to interact with this thing"

That command downloads Ubuntu, starts a container, and drops you at
a bash prompt *inside* it. Type `exit` to leave. The container stops
when you exit.

```bash
docker run -d --name myweb -p 8080:80 nginx
```

- `-d` — detached: run in the background, give me my terminal back
- `--name myweb` — call it `myweb` instead of a random name
  (Docker's random names like `jolly_morse` are charming once)
- `-p 8080:80` — publish port 80 *inside* the container as port 8080
  *on your machine* (more in [Ports and Networking](ports-and-networking.md))

**Check:** After that command, visiting `http://localhost:8080` in
your browser shows the nginx welcome page.

```bash
docker run --rm hello-world
```

- `--rm` — delete the container when it exits. For throwaway runs,
  always `--rm`, or stopped containers pile up (see [Day-to-Day
  Docker](day-to-day.md)).

## Reading a run command left to right

```bash
docker run [options] IMAGE [command]
```

Everything before the image name configures *how* to run; the image
name says *what* to run; anything after is the command to run
*inside* (overriding the image's default).

**Next:** [Images vs. Containers](images-vs-containers.md)
