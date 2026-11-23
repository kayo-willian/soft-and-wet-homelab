# Roadmap — Soft & Wet → Go Beyond → Made in Heaven

[Português](pt-BR/roadmap.md) · [Documentation](README.md)

This is the roadmap defined for the personal homelab project. Each stage is a **technical objective**, not simply a hardware replacement. The names and boundaries are preserved from the original one-page project roadmap.

| Stage | Repository / identity | Environment | Scope | Commitment |
|---|---|---|---|---|
| **01 — Initial** | `soft-and-wet-homelab` | Positivo, Debian 13, 2 GB RAM | First headless server; SSH, Docker/Compose, Navidrome and small useful services; document measurements and failures | **Current** |
| **02 — Parallel experiment** | `soft-and-wet-plunder` | Same Positivo, Alpine Linux | Explore a smaller distribution, OpenRC, containers and minimal dependencies; preserve data before replacing the OS | Optional experiment, not simultaneous with Debian |
| **03 — Conditional upgrade** | `soft-and-wet-homelab-v2` | Better notebook, OS undecided | Compare Ubuntu Server or UmbrelOS if compatible; expand useful self-hosted services | Only if new hardware becomes available |
| **04 — Optional stage** | `soft-and-wet-homelab-v3` | Hardware and OS undecided | New architecture or meaningful challenge | Can be skipped |
| **05 — Major leap** | `go-beyond` | Reused i7-3770 desktop | When the main desktop is replaced, repurpose the current one as a dedicated server; improve storage, networking, backup and monitoring | Future |
| **06 — Long-term vision** | `made-in-heaven` | Multiple devices; undefined hardware | NAS, virtualization, segmentation, redundancy and more mature personal infrastructure | No deadline |

## Current stage: Soft & Wet

The first milestone is a **working Navidrome + Tailscale music service**, which has been tested on the local browser and a remote Android client. That does not mean the whole homelab is complete: backups, access hardening, additional services and measured long-term operations remain open.

## Important constraints

- `soft-and-wet-plunder` is a **separate Alpine/OpenRC experiment**, not a currently installed second OS on the Positivo. Back up user data before replacing Debian.
- V2 and V3 refer to potential distinct repositories and environments, **not** deployed versions.
- The current i7-3770 desktop is the user's main PC and should only be reused for `go-beyond` when a replacement is available.
- No dates, funding commitments or obligation to create all six repositories.
- This roadmap is a **direction of study**, not a product release schedule.

The identity of the series is **Soft & Wet → Go Beyond → Made in Heaven**, with flexible intermediate experiments.
