# Soft & Wet — Debian Self-Hosted Homelab

[Versão em português](README.pt-BR.md)

A small, headless homelab running on a low-resource notebook. I use it to practice Linux administration, Docker Compose, networking, remote access, and the operation of self-hosted services. The emphasis is on **what was actually deployed and measured**, not an idealized stack.

## Hardware and environment

| Component | Configuration |
| --- | --- |
| Host | Positivo notebook, Intel Atom x5-Z8350 |
| Memory | 2 GB installed (approximately 1.8 GiB reported by Linux) |
| Internal storage | Approximately 29 GB nominal; 27 GB root filesystem |
| Operating system | Debian 13 (Trixie), headless |
| Hostname | `debian` |
| Administration | OpenSSH over the local network |
| Container runtime | Docker Engine + Docker Compose |
| Media storage | Internal eMMC only; no permanently attached USB drive |

## Current services

| Component | Deployment | Status |
| --- | --- | --- |
| Navidrome | Docker Compose | Running; local browser access and music library tested |
| Tailscale | Debian host + Android phone | Connected; remote mobile access tested |
| Portainer | — | Not installed |
| Uptime Kuma | — | Not installed |
| Heimdall | — | Not installed |
| Pi-hole / Apache | — | Not installed; only considered |

Navidrome holds a small, manually curated **Brasil** collection (10 tracks). Music files, database contents, credentials, and runtime data are **not tracked** in Git. Tailscale provides private connectivity between the Debian server and an Android phone; there is no need to forward Navidrome's port on the router.

## How it works

```text
Ubuntu desktop ── SSH / LAN ──> Debian 13 (Positivo)
                               ├── Docker Compose
                               │   └── Navidrome :4533
                               │       ├── /data  (database/cache)
                               │       └── /music (read-only library)
                               └── Tailscale (host installation)
                                       ⇅ encrypted tailnet
                                  Android + music client
```

The currently deployed Compose configuration is in [`compose.yaml`](compose.yaml). The commands and observations are recorded separately, including what has **not** been verified.

## Documentation

| Document | Purpose |
| --- | --- |
| [Architecture](docs/architecture.md) | Hardware, design decisions, storage and limitations |
| [Installation](docs/installation.md) | Initial Debian, Docker and directory setup |
| [Navidrome](docs/services/navidrome.md) | Actual Compose configuration, library, first run and tests |
| [Tailscale](docs/services/tailscale.md) | Host-side installation and mobile access |
| [Networking](docs/networking.md) | LAN, SSH, Docker ports and tailnet topology |
| [Music library](docs/music-library.md) | Tracklist of the first Brasil collection |
| [Measurements](docs/measurements.md) | Captured RAM, disk and container metrics |
| [Operations](docs/operations.md) | Routine checks, transfers and backup considerations |
| [Security](docs/security.md) | Current safeguards and unresolved items |
| [Troubleshooting](docs/troubleshooting.md) | Problems encountered and diagnostics |
| [Changelog](CHANGELOG.md) | Recorded deployment milestones |

## Reproduce the Navidrome container

On a prepared Debian host with Docker and the Compose plugin installed:

```bash
mkdir -p ~/soft-and-wet/{data/navidrome,music,docs,scripts}
cd ~/soft-and-wet
# Copy compose.yaml and .env.example from this repository into this directory.
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
sudo docker compose config
sudo docker compose up -d
sudo docker compose ps
```

Use the instructions in [installation](docs/installation.md) for a clean host. The Compose file is the **original deployed version**, not a hardened redesign. No music is included.

## Scope

This is a resource-constrained, single-host learning environment, not a high-availability or internet-facing production service. I add services only after checking memory and storage usage. **Portainer and monitoring tools have not been deployed yet.**

## Upstream references

[Debian](https://www.debian.org/) · [Docker Engine](https://docs.docker.com/engine/install/debian/) · [Docker Compose](https://docs.docker.com/compose/) · [Navidrome](https://www.navidrome.org/docs/installation/docker/) · [Tailscale](https://tailscale.com/docs/install/linux)
