# What Docker Is

**The one-sentence version:** Docker packages your application with
everything it needs to run — the language runtime, libraries, settings
— into a **container**: a sealed, portable box that behaves the same
on your laptop, your friend's laptop, and a server.

## The problem it solves

You've installed a program and hit: wrong Python version, missing
library, "works on my machine." Every computer is a unique snowflake
of installed software, and applications break on the differences.

The old fix was a **virtual machine**: emulate an entire computer,
OS and all, per application. It works, but it's heavy — each VM
carries a full operating system, boots in minutes, and eats gigabytes.

Docker's insight: the containers don't each need their own OS. They
share the host's kernel and just *think* they have their own machine.
Same isolation idea, a fraction of the weight: containers start in
milliseconds and share everything they can.

| | Virtual machine | Container |
|---|---|---|
| What's inside | Full guest OS | Just your app + its needs |
| Start time | Minutes | Milliseconds to seconds |
| Size | Gigabytes | Megabytes |
| Isolation | Very strong (own kernel) | Strong (shared kernel) |

## The shipping-container analogy

Before standardized shipping containers, cargo was loaded by hand —
every port, every ship, a custom job. Then came the metal box: the
*contents* differ, but the *handling* is identical everywhere. Cranes,
ships, and trucks only need to know "it's a container."

Docker is that for software. The app inside differs; the handling —
build it, ship it, run it, stop it — is identical everywhere. That's
why a container that runs on your laptop runs on a server: the
"crane" (the Docker engine) is the same.

## The three things to learn

1. **Image** — the sealed package (the recipe + ingredients).
   Read-only, shareable, versioned.
2. **Container** — a *running instance* of an image. Disposable by
   design.
3. **Dockerfile** — the text file describing how to build an image.

Everything else in this guide hangs off those three.

## What Docker is not

- **Not a virtual machine.** No guest OS per container; they share
  your kernel. (That's also why a Linux container doesn't run
  natively on Windows — Docker Desktop runs a hidden Linux VM
  underneath to provide the kernel.)
- **Not magic security.** Containers isolate processes well, but a
  container running as root with access to the host is a risk, not a
  sandbox. Don't run untrusted images blindly.
- **Not the whole story.** Docker runs containers on *one* machine.
  Running them across *many* machines is orchestration (Kubernetes) —
  a separate subject.

**Next:** [Installing Docker](installing-docker.md)
