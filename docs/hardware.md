# Hardware inventory — three roles, one small server

[Português (Brasil)](pt-BR/inventario.md) · [Documentation](README.md) · [Home](../README.md)

This inventory records the **devices actually involved** in Soft & Wet. The Positivo is the server. The Ubuntu PC is an administration and file-transfer workstation. The Android device is a remote client. They are not a cluster, and neither of the latter two is a Docker host for this project.

## 1. Physical server — Positivo

| Component | Recorded value |
| --- | --- |
| Role | **Soft & Wet server** |
| Device | Positivo notebook |
| Processor | **Intel Atom x5-Z8350** |
| Installed RAM | **2 GB** (~1.8 GiB shown by Linux) |
| Internal storage | ~29 GB advertised; **27 GiB** root filesystem |
| Root block device | `/dev/mmcblk2p2` |
| Swap partition | `/dev/mmcblk2p3`, **1.5 GiB** |
| OS | **Debian 13 (Trixie)** without a desktop |
| Hostname | `debian` |
| Management | OpenSSH; headless operation in another room |
| Application runtime | Docker Engine and **Docker Compose v5.8.0** as reported on first measurement |
| Persistent media | Internal eMMC only; **no permanently attached Ventoy USB** |

The initial `df -h /` reading showed **27 GiB total, 2.0 GiB used, 23 GiB available**. The first `free -h` sample showed **388 MiB in use and ~1.4 GiB available**; swap usage was zero. Those numbers describe the state **before** the first application container, not the current disk/memory use.

The design does not assume redundant drives, a UPS, external backup storage or a dedicated graphics accelerator. The machine may be reachable with the lid closed after configuration, but the exact power-management state was not independently verified.

## 2. Administration workstation — Ubuntu desktop

The workstation's measurements below come from a `./info.sh`, `ip a`, `df -h /` and `free -h` snapshot dated **2026-11-23**.

| Component | Captured specification |
| --- | --- |
| Role | SSH administration, browser access, file organization, `rsync` |
| Processor | **Intel Core i7-3770**, 4 physical cores / **8 threads** |
| Graphics | **Intel HD Graphics 4000** integrated |
| Memory | ~**16 GB installed**, **14.83 GiB reported** by the system-info utility |
| OS | **Ubuntu 26.04.1 LTS** |
| Kernel | **Linux 7.0.0-44-generic** |
| Desktop | **GNOME 50.1**, Mutter on **Wayland** |
| Shell | **Bash 5.3.9** |
| Root partition | `/dev/sda2`, **~109.47 GiB** reported by system-info utility |
| Root usage snapshot | ~**33.53 GiB used**, ~**71 GiB available** (from `df`) |
| Swap | **4.00 GiB**, 0 B in use when measured |
| Connectivity | Ethernet and USB Wi-Fi connected to the home LAN; exact addresses omitted |

The different rounding of disk and RAM values reflects different units and command formats, not additional physical devices. The system inventory is a **point-in-time snapshot**, not a guarantee of today's installed kernel, memory utilization or disk space.

### Why the workstation matters

- SSH administers Debian without a monitor or local session.
- `rsync` transfers the curated music library from `~/Músicas/Brasil/` to the server.
- A browser verifies locally hosted services.
- The desktop's stronger CPU/RAM **do not add resources to the Positivo**. Soft & Wet remains limited to its own 2 GB RAM and eMMC.

## 3. Mobile client — Samsung Galaxy A34 5G

| Component | Specification |
| --- | --- |
| Role | Mobile client accessing the self-hosted services |
| Model | **Samsung Galaxy A34 5G** |
| Variant owned | **6 GB RAM / 128 GB storage** |
| Processor | **MediaTek Dimensity 1080** (model specification) |
| Screen | **6.6″ Super AMOLED, 120 Hz** |
| Battery | **5,000 mAh** |
| Network | Wi-Fi and **5G** mobile data |
| Access tested | Tailscale tailnet + a Navidrome-compatible music application |

The model specifications are supported by [Samsung Mobile Press](https://samsungmobilepress.com/media-assets/galaxy-a34-5g?tab=specs) and [Samsung Newsroom Brazil](https://news.samsung.com/br/samsung-galaxy-a34-5g-esta-disponivel-em-todo-o-brasil). My phone is the 6 GB / 128 GB variant. I confirmed remote playback separately from the terminal measurements. An exact Android/One UI version was **not provided** and is not assumed here.

## Network and publication boundary

LAN, Docker bridge, Tailscale and MAC addresses from the raw terminal output are **deliberately excluded** from this public inventory. The topology is described in [networking](networking.md); it can be reproduced using your own devices and addresses.

See [architecture](architecture.md) for how the roles fit together and [measurements](measurements.md) for the captured server workload.
