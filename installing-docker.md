# Installing Docker

## The easy path: Docker Desktop (Mac and Windows)

Download [Docker Desktop](https://www.docker.com/products/docker-desktop/),
install it, launch it. It includes the Docker engine, the Compose
plugin, and a GUI. On Windows it needs WSL2 (it will tell you).

Docker Desktop is free for personal use and small businesses; larger
companies need a paid subscription. Check the current terms — they've
changed before.

## Linux: from your distro's repository

On Debian/Ubuntu/Mint:

```bash
sudo apt update && sudo apt install docker.io docker-compose-plugin
```

(Docker also publishes its own repository with newer versions; the
distro package is simpler and fine for learning.)

Fedora: `sudo dnf install docker docker-compose-plugin`. Arch:
`sudo pacman -S docker docker-compose`.

Then start the service and enable it at boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## The post-install step everyone skips (Linux)

By default, `docker` needs root. Typing `sudo` before every command
gets old, so add yourself to the `docker` group:

```bash
sudo usermod -aG docker $USER
```

Then **log out and back in** — group membership only applies to new
logins.

**Careful:** The `docker` group is effectively root-equivalent.
Anyone in it can mount the host's filesystem into a container and
read anything. Only add users you trust as administrators — which, on
your own machine, is you.

## Verify it works

```bash
docker --version
docker compose version
```

**Check:** You see version numbers, e.g. `Docker version 29.1.3`
and `Docker Compose version v5.6.0`. (Yours will differ — that's
fine.)

```bash
docker run hello-world
```

This downloads a tiny test image and runs it. **Check (typical
output, abbreviated):**

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

If you see that, the engine, image downloads, and container startup
all work.

**Next:** [Your First Container](your-first-container.md)
