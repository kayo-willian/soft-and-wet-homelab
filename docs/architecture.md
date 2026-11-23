# Architecture

## Purpose

Soft & Wet is a self-hosted lab with a hard resource budget. It runs on a Positivo notebook with an Intel Atom x5-Z8350, 2 GB of physical RAM and a small internal eMMC. The host uses Debian 13 without a graphical desktop; administrative work is performed over SSH.

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

- `~/soft-and-wet/compose.yaml`: Compose definition.
- `~/soft-and-wet/.env`: host UID/GID, not committed.
- `~/soft-and-wet/data/navidrome/`: application database/cache, not committed.
- `~/soft-and-wet/music/Brasil/`: ten music files, not committed.

## Container model

The current application stack has **one** container, `navidrome`, connected to `soft-and-wet_default`. Port `4533/tcp` is published on the host. Tailscale is **not** a container; it runs as a host service.

## Constraints

- No graphical session on the host.
- Low memory and limited internal storage; no heavy VM or large downloaded media library.
- All new services require a new resource measurement.
- No assumption of external backups or high availability.

See [measurements](measurements.md) and [security](security.md) for observed figures and open issues.
