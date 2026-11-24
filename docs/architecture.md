# Architecture

[Português (Brasil)](pt-BR/arquitetura.md) · [Documentation](README.md) · [Home](../README.md)

## Purpose

Soft & Wet is a self-hosted lab with a hard resource budget. It runs on a Positivo notebook with an Intel Atom x5-Z8350, 2 GB of physical RAM and a small internal eMMC. The host uses Debian 13 without a graphical desktop; administrative work is performed over SSH.

A full three-device inventory is available in [hardware](hardware.md). The Ubuntu workstation and the Android phone are clients, not additional homelab servers.

## Deployed topology

```text
                       Local network
     Ubuntu PC ------------------------------+
     SSH client / file source                |
                                            v
                                 Positivo — Debian 13
                                 hostname: debian
                                 - OpenSSH (host)
                                 - Docker Engine (host)
                                 - Tailscale (host)
                                       |
                                 Docker Compose
                                       |
                              soft-and-wet_default
                                       |
                                navidrome :4533
                                /data  read/write
                                /music read-only
                                       ^
                                       |
                              internal eMMC storage

                              Tailscale private network
                          Debian <================> Android
                                  encrypted tunnel
                                      music client
```

## Storage arrangement

The operating system, Docker data, Navidrome database/cache, and music collection are on internal storage. An earlier plan to use a USB drive with Ventoy was **abandoned**; there is no permanently connected USB storage in the current design.

| Path on Debian | Contents | Published? |
| --- | --- | --- |
| `~/soft-and-wet/compose.yaml` | Compose definition | Yes |
| `~/soft-and-wet/.env` | Local UID/GID | No |
| `~/soft-and-wet/data/navidrome/` | Database and cache | No |
| `~/soft-and-wet/music/Brasil/` | Ten music files | No |

The GitHub repository is named `soft-and-wet-homelab`. The original deployment directory on Debian is `~/soft-and-wet`.

## Container model

The current application stack has **one** container, `navidrome`, connected to `soft-and-wet_default`. Port `4533/tcp` is published on the host. Tailscale is **not** a container; it runs as a host service.

## Constraints

- No graphical session, permanent USB drive or movie download automation on the host.
- Low memory and limited internal storage; no heavy VM or large downloaded media library.
- All new services require a new resource measurement.
- No assumption of external backups or high availability.

Docker publishes TCP 4533 on all host interfaces. See [networking](networking.md), [measurements](measurements.md) and [security](security.md) for the recorded configuration, resource use and open issues.
