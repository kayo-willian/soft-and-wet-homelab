# Services — installed and planned

[Português (Brasil)](../pt-BR/servicos/README.md) · [Documentation](../README.md) · [Home](../../README.md)

Soft & Wet is a **personal infrastructure project**, not a bundle of ready-made services. The services below must fit an Atom x5-Z8350, 2 GB of RAM and limited eMMC storage.

| Component | Layer | Status | Why |
| --- | --- | --- | --- |
| [Debian 13](https://www.debian.org/) | Host OS | Running | Headless, lightweight, managed via SSH |
| [OpenSSH](https://www.openssh.com/) | Host | Running | Administration and encrypted file transfers |
| [Docker Engine](https://docs.docker.com/engine/) + [Compose](https://docs.docker.com/compose/) | Host | Running | Application isolation and reproducible configuration |
| [Navidrome](https://www.navidrome.org/) | Container | **Running** | Personal music library |
| [Tailscale](https://tailscale.com/) | Host + Android | **Working** | Private remote connectivity without router forwarding |
| [Portainer CE](https://www.portainer.io/) | Container | **Not installed** | Web management of Docker; costs RAM and grants privileged access to Docker |
| [Uptime Kuma](https://github.com/louislam/uptime-kuma) | Container | **Not installed** | Availability and latency monitoring |
| [Heimdall](https://heimdall.site/) | Container | **Not installed** | Optional launchpad; not operationally required |
| [Pi-hole](https://pi-hole.net/) | Container | **Not installed** | Optional DNS filtering, requires network care |
| [Apache HTTP Server](https://httpd.apache.org/) | Container if tried | **Not installed** | Optional HTTP/proxy learning |
| [Beszel](https://beszel.dev/) | Monitoring | **Not installed** | Optional CPU/RAM/disk metrics |
| [Dozzle](https://dozzle.dev/) | Logging | **Not installed** | Optional live Docker log viewer |

## Current service details

- [Navidrome](navidrome.md) — exactly one deployed application container, its Compose definition, library and first measurements.
- [Tailscale](tailscale.md) — runs natively on the Debian host and on Android, **not** in Docker.

## Deliberately excluded or deferred

- Plex and Jellyfin: overlap with the music-focused Navidrome use case.
- Sonarr, Radarr, Prowlarr, Bazarr and torrent/video download automation: unnecessary storage and operational overhead.
- File Browser: discussed early, but the original upstream repository was archived; not selected for this deployment.
- Samba, Syncthing, Gatus and zram: considered as possible additions or alternatives, **not implemented**.
- Portainer, Uptime Kuma, Heimdall, Pi-hole, Apache, Beszel and Dozzle remain suggestions until independently deployed and measured.

## Decision rule

Add one service at a time. Record Docker image footprint, idle RAM, CPU under use, occupied disk, persistent paths and networking changes. Only then decide if it should remain.

See [measurements](../measurements.md), [security](../security.md), and [operations](../operations.md).
