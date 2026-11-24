<div align="center">

![Soft & Wet — manga-inspired homelab cover](assets/soft-and-wet-cover.webp)

# Soft & Wet

[Português (Brasil)](README.pt-BR.md) · [Documentation](docs/README.md) · [Home](README.md)

**A real home server built from a nearly obsolete laptop.**

[![License MIT](https://img.shields.io/static/v1?label=License&message=MIT&color=A0A0A0&style=flat-square&labelColor=161B22)](LICENSE)

**In use in the lab**

[![Debian 13](https://img.shields.io/static/v1?label=Debian&message=13&color=A81D33&style=flat-square&labelColor=161B22&logo=debian&logoColor=white)](https://www.debian.org/) [![Linux Server](https://img.shields.io/static/v1?label=Linux&message=Server&color=D6B400&style=flat-square&labelColor=161B22&logo=linux&logoColor=white)](https://kernel.org/) [![Ubuntu 26.04 LTS](https://img.shields.io/static/v1?label=Ubuntu&message=26.04%20LTS&color=E95420&style=flat-square&labelColor=161B22&logo=ubuntu&logoColor=white)](https://ubuntu.com/) [![Android Client](https://img.shields.io/static/v1?label=Android&message=Client&color=3DDC84&style=flat-square&labelColor=161B22&logo=android&logoColor=white)](https://www.android.com/) [![OpenSSH Remote](https://img.shields.io/static/v1?label=OpenSSH&message=Remote&color=527EA2&style=flat-square&labelColor=161B22)](https://www.openssh.com/) [![Docker Engine](https://img.shields.io/static/v1?label=Docker&message=Engine&color=2496ED&style=flat-square&labelColor=161B22&logo=docker&logoColor=white)](https://www.docker.com/) [![Docker Compose](https://img.shields.io/static/v1?label=Docker&message=Compose&color=1D63ED&style=flat-square&labelColor=161B22&logo=docker&logoColor=white)](https://docs.docker.com/compose/) [![Navidrome Audio](https://img.shields.io/static/v1?label=Navidrome&message=Audio&color=27AFA2&style=flat-square&labelColor=161B22)](https://www.navidrome.org/) [![Tailscale Access](https://img.shields.io/static/v1?label=Tailscale&message=Access&color=333333&style=flat-square&labelColor=161B22)](https://tailscale.com/) [![Git Versioning](https://img.shields.io/static/v1?label=Git&message=Versioning&color=F05032&style=flat-square&labelColor=161B22&logo=git&logoColor=white)](https://git-scm.com/) [![Bash Shell](https://img.shields.io/static/v1?label=Bash&message=Shell&color=4EAA25&style=flat-square&labelColor=161B22&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/) [![rsync Transfer](https://img.shields.io/static/v1?label=rsync&message=Transfer&color=5C677D&style=flat-square&labelColor=161B22)](https://rsync.samba.org/)

**Tools considered or awaiting configuration**

[![Portainer CE](https://img.shields.io/static/v1?label=Portainer&message=CE&color=13BEF9&style=flat-square&labelColor=161B22)](https://www.portainer.io/) [![Heimdall Dashboard](https://img.shields.io/static/v1?label=Heimdall&message=Dashboard&color=9B64C6&style=flat-square&labelColor=161B22)](https://heimdall.site/) [![Uptime Kuma Monitoring](https://img.shields.io/static/v1?label=Uptime%20Kuma&message=Monitoring&color=4ABF76&style=flat-square&labelColor=161B22)](https://github.com/louislam/uptime-kuma) [![Pi-hole DNS](https://img.shields.io/static/v1?label=Pi-hole&message=DNS&color=96060C&style=flat-square&labelColor=161B22)](https://pi-hole.net/) [![Apache HTTP Server](https://img.shields.io/static/v1?label=Apache&message=HTTP%20Server&color=D22128&style=flat-square&labelColor=161B22&logo=apache&logoColor=white)](https://httpd.apache.org/) [![Beszel Metrics](https://img.shields.io/static/v1?label=Beszel&message=Metrics&color=168A72&style=flat-square&labelColor=161B22)](https://beszel.dev/) [![Dozzle Logs](https://img.shields.io/static/v1?label=Dozzle&message=Logs&color=8266CF&style=flat-square&labelColor=161B22)](https://dozzle.dev/) [![Samba Files](https://img.shields.io/static/v1?label=Samba&message=Files&color=087E8B&style=flat-square&labelColor=161B22)](https://www.samba.org/) [![Syncthing Sync](https://img.shields.io/static/v1?label=Syncthing&message=Sync&color=0891D1&style=flat-square&labelColor=161B22&logo=syncthing&logoColor=white)](https://syncthing.net/) [![Gatus Status](https://img.shields.io/static/v1?label=Gatus&message=Status&color=CD583D&style=flat-square&labelColor=161B22)](https://github.com/TwiN/gatus) [![SFTP Transfer](https://img.shields.io/static/v1?label=SFTP&message=Transfer&color=758C49&style=flat-square&labelColor=161B22)](https://www.openssh.com/) [![UFW Firewall](https://img.shields.io/static/v1?label=UFW&message=Firewall&color=B56E28&style=flat-square&labelColor=161B22)](https://wiki.ubuntu.com/UncomplicatedFirewall) [![Fail2ban Defense](https://img.shields.io/static/v1?label=Fail2ban&message=Defense&color=B02E5E&style=flat-square&labelColor=161B22)](https://github.com/fail2ban/fail2ban)

The [service registry](docs/services/README.md) records individual service states. I used `rsync` to transfer music. Backups and active UFW and Fail2ban policies remain unverified.

</div>

---

## The experiment

I had a Positivo notebook with **2 GB of RAM**, an Intel Atom processor and roughly **29 GB of internal eMMC**. No spare server, no NAS, no room for heavy virtualization, and no reason to leave another computer occupying my desk.

Instead of discarding it, I installed **Debian 13 without a graphical environment**, moved it to another room, and started running services I could administer over SSH from my Ubuntu PC and access from my phone.

**The project is about making limited hardware genuinely useful.** Containers, streaming and remote access are ways of testing that idea—not the purpose of the repository. I want to understand the host, measure what each service costs, troubleshoot failures, document the trade-offs, and expand only when the hardware can handle it.

The name and artwork take inspiration from *JoJolion*. Like the early experiments, this homelab starts small and leaves room for the unexpected.

## The three devices

| Role | Machine | Important specifications |
| --- | --- | --- |
| **Server — Soft & Wet** | Positivo notebook | Intel Atom x5-Z8350 · **2 GB RAM** · ~29 GB internal eMMC · **Debian 13**, headless |
| **Administration / file source** | Ubuntu desktop | **Intel Core i7-3770 (4 cores / 8 threads)** · **Intel HD Graphics 4000** · ~16 GB RAM · Ubuntu **26.04.1 LTS** · GNOME 50.1 / Wayland |
| **Remote mobile client** | Samsung Galaxy A34 5G | **6 GB RAM / 128 GB storage** · 6.6″ Super AMOLED 120 Hz · 5G · 5,000 mAh battery |

The desktop is **not** the server; it provides SSH administration, file transfer and browser-based access. The Android phone is a remote client. Full, source-annotated specifications—including the desktop's kernel and disk snapshot—are in [hardware inventory](docs/hardware.md).

## What I actually did

| Milestone | Implementation | Evidence |
| --- | --- | --- |
| Installed a minimal OS | Debian 13, no desktop environment | Server operational over SSH from another room |
| Set up remote administration | OpenSSH and standard Linux tools | Managed from Ubuntu |
| Introduced containers | Docker Engine + Compose | Compose reported v5.8.0 |
| Validated a real workload | Navidrome and ten locally stored songs | Browser playback/library confirmed |
| Tested remote access | Tailscale on Debian and Android | Streaming outside home network confirmed |
| Captured a baseline | `df`, `free`, `swapon`, `docker stats` | 23 GiB free before apps; 388 MiB host RAM used |

Navidrome is the only documented application container. Tailscale runs directly on Debian. Expansion is paused. The [service registry](docs/services/README.md) lists the tools considered for later use.

## How the lab is connected

```text
Ubuntu desktop (SSH, rsync, browser)
       │ home LAN
       ▼
Positivo · Debian 13 · internal eMMC
       ├── OpenSSH
       ├── Docker Engine + Compose → Navidrome (TCP 4533)
       └── Tailscale
              │ private tailnet
              ▼
       Galaxy A34 5G (remote playback)
```

Remote playback worked without port forwarding on the router. Docker publishes TCP port **4533** on all host interfaces. Restricting LAN access remains open.

## Resource budget

| Baseline / first observation | Recorded result |
| --- | --- |
| Root filesystem before app deployment | 27 GiB total · 2.0 GiB used · **23 GiB available** |
| Host RAM before app deployment | ~1.8 GiB visible · **388 MiB used** · ~1.4 GiB available |
| Swap | 1.5 GiB partition · 0 B used |
| First Navidrome container reading | **32.51 MiB RAM** · 15.83% CPU at startup (one instant) |
| External storage | **None** — USB Ventoy idea rejected for permanent operation |

The [first Navidrome reading](docs/measurements.md#navidrome-first-startup) was taken at startup. Music, metadata, container images and logs share the eMMC with Debian. I measure resource use before adding software.

## Reproduce and understand

On Debian, use a regular account with Git, sudo, Docker Engine and the Compose plugin. Run this once for a fresh deployment where `~/soft-and-wet` does not yet exist. The command creates a local `.env` with the IDs of the account that owns the bind mounts.

```bash
git clone https://github.com/kayo-willian/soft-and-wet-homelab.git ~/soft-and-wet &&
cd ~/soft-and-wet &&
mkdir -p data/navidrome music &&
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env &&
sudo docker compose config &&
sudo docker compose up -d &&
sudo docker compose ps
```

This starts Navidrome with the [original Compose configuration](compose.yaml). Its database and music stay on the host. The [installation notes](docs/installation.md) describe the first deployment.

## Technical documentation

| Area | Document |
| --- | --- |
| Navigation | [Documentation index](docs/README.md) |
| Physical devices | [Hardware inventory](docs/hardware.md) |
| Host architecture | [Architecture](docs/architecture.md) |
| Install and reproduce | [Installation](docs/installation.md) |
| Services and containers | [Service registry](docs/services/README.md) |
| Services | [Navidrome](docs/services/navidrome.md) |
| Services | [Tailscale](docs/services/tailscale.md) |
| Network | [Network and remote access](docs/networking.md) |
| Operations | [Maintenance and backup](docs/operations.md) |
| Security | [Security boundaries](docs/security.md) |
| Measurements | [Real measurements](docs/measurements.md) |
| Media catalog | [Brasil collection](docs/music-library.md) |
| Failures and decisions | [Troubleshooting](docs/troubleshooting.md) |
| Evolution | [Roadmap](docs/roadmap.md) |
| Milestones | [Changelog](CHANGELOG.md) |
| Automation | [Scripts](scripts/README.md) |

The [security notes](docs/security.md) explain which local data to keep out of Git.

## What comes next?

This is still the beginning. The public names belong to a larger, deliberately unrevealed sequence:

<div align="center">

**SOFT & WET**  
↓  
**PLUNDER**  
↓  
**SOFT & WET · V2**  
↓  
**SOFT & WET · V3**  
↓  
**GO BEYOND**  
↓  
**MADE IN HEAVEN**

*Some stages may never happen. The details stay hidden until there is something real to show.*

</div>

## Scope and license

This is a personal technical project—not a software product, commercial service or enterprise environment. No VM, no permanent removable drive, no movie download automation, and no claims of tested backups, hardened public exposure or availability guarantees.

The [MIT license](LICENSE) covers this repository's original documentation and configuration, not third-party software or music files.
