<div align="center">

![Soft & Wet cover](assets/soft-and-wet-cover.svg)

# Soft & Wet
### Debian 13 · Self-hosted homelab

[![Português](https://img.shields.io/badge/READ%20IN-PORTUGU%C3%8AS-0D0D0D?style=for-the-badge&labelColor=2F81F7)](README.pt-BR.md)
[![Status](https://img.shields.io/badge/STATUS-ACTIVE%20LAB-0D0D0D?style=for-the-badge&labelColor=2F81F7)](CHANGELOG.md)
[![Platform](https://img.shields.io/badge/DEBIAN-13-0D0D0D?style=for-the-badge&logo=debian&logoColor=2F81F7)](https://www.debian.org/)
[![Docker](https://img.shields.io/badge/DOCKER-COMPOSE-0D0D0D?style=for-the-badge&logo=docker&logoColor=2F81F7)](https://docs.docker.com/compose/)
[![Access](https://img.shields.io/badge/ACCESS-SSH%20%2B%20TAILSCALE-0D0D0D?style=for-the-badge&logo=tailscale&logoColor=2F81F7)](https://tailscale.com/)
[![Memory](https://img.shields.io/badge/HOST%20RAM-2%20GB-0D0D0D?style=for-the-badge&labelColor=2F81F7)](docs/measurements.md)
[![License](https://img.shields.io/badge/LICENSE-MIT-0D0D0D?style=for-the-badge&labelColor=2F81F7)](LICENSE)

**A personal infrastructure project built on a small, physical notebook — not a packaged application.**

[Architecture](docs/architecture.md) · [Deployment](docs/installation.md) · [Services](docs/services/README.md) · [Troubleshooting](docs/troubleshooting.md) · [Full documentation](docs/README.md)

</div>

---

## Why I built it

I want a real server that I can operate from another room, break, troubleshoot and improve — using the hardware I already have. Soft & Wet is my first self-hosted environment. I use it to practice Debian administration, SSH, Docker, networks, file organization and private remote access. I document what actually works, what failed, and what I have not attempted.

The name is inspired by **Soft & Wet** from *JoJolion*. The original cover artwork uses a manga-inspired palette, bubbles and the server's small physical footprint.

## Working implementation

| Component | Actual deployment | Verification |
|---|---|---|
| **Debian 13 / OpenSSH** | Headless Positivo host | Managed remotely from Ubuntu via SSH |
| **Docker Engine + Compose** | Host runtime | Compose v5.8.0 reported; first container started |
| **Navidrome** | One Docker container, TCP 4533 | Web UI and ten-track library loaded |
| **Tailscale** | Native host service + Android app | Music streaming confirmed away from home |
| **Brasil library** | Ten manually selected songs on internal eMMC | Visible in Navidrome's Songs view |
| Portainer / Uptime Kuma / Heimdall | Not installed | Future evaluation, subject to RAM/disk limits |

<div align="center">

### Services and upstream documentation

[![Navidrome](https://img.shields.io/badge/NAVIDROME-MUSIC%20SERVER-0D0D0D?style=for-the-badge&labelColor=2F81F7)](https://www.navidrome.org/)
[![Tailscale](https://img.shields.io/badge/TAILSCALE-PRIVATE%20ACCESS-0D0D0D?style=for-the-badge&logo=tailscale&logoColor=2F81F7)](https://tailscale.com/)
[![Portainer planned](https://img.shields.io/badge/PORTAINER-PLANNED-0D0D0D?style=for-the-badge&logo=portainer&logoColor=8B99AB)](https://www.portainer.io/)
[![Uptime Kuma planned](https://img.shields.io/badge/UPTIME%20KUMA-PLANNED-0D0D0D?style=for-the-badge&logo=uptimekuma&logoColor=8B99AB)](https://github.com/louislam/uptime-kuma)
[![Heimdall planned](https://img.shields.io/badge/HEIMDALL-PLANNED-0D0D0D?style=for-the-badge&logo=linux&logoColor=8B99AB)](https://heimdall.site/)

</div>

## Physical host

| Hardware / system | Specification |
|---|---|
| Device | Positivo notebook |
| CPU | Intel Atom x5-Z8350 |
| RAM | 2 GB physical (~1.8 GiB visible to Linux) |
| Storage | Internal eMMC only; 27G root filesystem |
| OS | Debian 13 (Trixie), no desktop |
| Hostname | `debian` |
| Management | SSH from an Ubuntu desktop |
| Containers | Docker Engine with Docker Compose |
| Remote client | Samsung A34 with Tailscale and a compatible music client |

An external Ventoy USB stick was considered, **then explicitly excluded** from the permanent setup. Media and application data live on internal storage. This is intentionally a small, single-host system.

## Architecture

~~~text
Ubuntu PC ── SSH / rsync ── home LAN ──┐
                                      │
                              Positivo notebook
                               Debian 13 headless
                                ├── OpenSSH
                                ├── Docker Engine
                                │    └── Docker Compose
                                │          └── Navidrome :4533
                                │              ├── /data  (read/write)
                                │              └── /music (read-only)
                                └── Tailscale
                                      ║ private tailnet
                                      ╚══ Android + music client
~~~

Navidrome is the **only deployed application container**. The Compose network is `soft-and-wet_default`. Docker currently binds port 4533 to all host interfaces; private tailnet access does **not** by itself harden the LAN binding. See [security](docs/security.md) for the open issue.

## Reproduce the first container

On a prepared Debian 13 system with Docker/Compose:

~~~bash
git clone https://github.com/kayo-willian/soft-and-wet-homelab.git
cd soft-and-wet-homelab
mkdir -p data/navidrome music
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
sudo docker compose config
sudo docker compose up -d
sudo docker compose ps
~~~

This reproduces the Navidrome service, **not** the full original Debian installation or the tailnet authentication. No music, private credentials or application database is included. For the host preparation and remote access, follow [installation](docs/installation.md) and [Tailscale](docs/services/tailscale.md).

## Measurements, not marketing

| Captured observation | Value |
|---|---|
| Before first app: root filesystem | 27G total / 2.0G used / 23G available |
| Before first app: RAM | 388 MiB used; ~1.4 GiB available |
| Swap | 1.5 GiB partition; 0 used at measurement |
| Navidrome startup memory | 32.51 MiB (single sample) |
| Navidrome startup CPU | 15.83% (instantaneous, **not** average) |

There are no verified post-library or multi-service benchmarks yet. [Full measurements](docs/measurements.md).

## Technical documentation

The documentation mirrors the bilingual, categorized structure of my [Enterprise NOC — From Scratch](https://github.com/kayo-willian/enterprise-noc-from-scratch) project without borrowing its scale or claiming enterprise-level availability.

| Area | English | Português |
|---|---|---|
| Documentation index | [Index](docs/README.md) | [Índice](docs/pt-BR/README.md) |
| Architecture & storage | [Architecture](docs/architecture.md) | [Arquitetura](docs/pt-BR/architecture.md) |
| Reproduction | [Installation](docs/installation.md) | [Instalação](docs/pt-BR/installation.md) |
| Services | [Services](docs/services/README.md) | [Serviços](docs/pt-BR/services/README.md) |
| Networking & remote access | [Networking](docs/networking.md) | [Redes](docs/pt-BR/networking.md) |
| Operations & backup | [Operations](docs/operations.md) | [Operações](docs/pt-BR/operations.md) |
| Security | [Security](docs/security.md) | [Segurança](docs/pt-BR/security.md) |
| Resource evidence | [Measurements](docs/measurements.md) | [Medições](docs/pt-BR/measurements.md) |
| Music selection | [Music library](docs/music-library.md) | [Biblioteca](docs/pt-BR/music-library.md) |
| Decisions & failed attempts | [Troubleshooting](docs/troubleshooting.md) | [Solução de problemas](docs/pt-BR/troubleshooting.md) |
| Future stages | [Roadmap](docs/roadmap.md) | [Evolução](docs/pt-BR/roadmap.md) |

## Project evolution

This is the **first** stage of an open-ended personal infrastructure series, not six mandatory releases of the same application.

**Soft & Wet → Go Beyond → Made in Heaven**

~~~text
01 soft-and-wet-homelab       | Positivo · Debian 13        | CURRENT
02 soft-and-wet-plunder       | Positivo · Alpine/OpenRC    | Parallel experiment
03 soft-and-wet-homelab-v2    | Better notebook            | Conditional
04 soft-and-wet-homelab-v3    | New hardware/architecture  | Optional
05 go-beyond                 | i7-3770 recycled as server | Future
06 made-in-heaven            | Multi-device infrastructure| Long term
~~~

See the [full roadmap](docs/roadmap.md). Future phases have no deadlines or guarantee of implementation.

## Scope and limitations

- One real machine, no VM and no desktop environment.
- No film/series automation or continuously attached USB drive.
- Storage capacity, RAM and backups are explicit constraints.
- No claim of hardened production security, high availability, successful backups or deployed planned services.
- Personal music files, Navidrome state, `.env`, secrets and private addresses are not stored in Git.

## License

MIT — [LICENSE](LICENSE). The license applies to this repository's configuration and documentation, **not** to music files or upstream applications.
