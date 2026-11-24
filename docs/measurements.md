# Resource measurements

[Português (Brasil)](pt-BR/medicoes.md) · [Documentation](README.md) · [Home](../README.md)

Measurements were captured through SSH on the Debian host on **2026-11-23**. Values change with load and may not represent steady-state usage.

## Baseline: before Navidrome

| Measurement | Captured value |
| --- | --- |
| Root filesystem | `/dev/mmcblk2p2`, 27 GiB total, 2.0 GiB used, 23 GiB available, 8% used |
| Memory | 1.8 GiB total; 388 MiB used; 884 MiB free; 1.4 GiB available |
| Swap | 1.5 GiB total; 0 B used |
| Swap device | `/dev/mmcblk2p3`, partition |
| Docker images | 1, 36.94 kB reported |
| Running containers | 0 |
| Local Docker volumes | 0 |
| Docker build cache | 0 |
| Docker Compose version | `v5.8.0` |

Captured with:

```bash
df -h /
free -h
sudo docker system df
sudo docker compose version
sudo /usr/sbin/swapon --show
```

## Navidrome first startup

The first `docker stats --no-stream` sample, taken shortly after container creation, reported:

| Metric | Observed |
| --- | --- |
| Container | `navidrome` |
| Memory | **32.51 MiB** (1.77% of reported limit) |
| CPU | **15.83%** (instantaneous startup sample) |
| PIDs | 13 |
| State | Running |
| Published port | TCP 4533 |

This is **not** a long-term average or a guarantee of future RAM use. Music indexing and playback may change consumption.

## Missing measurements

- Post-library disk usage and steady-state Navidrome memory.
- Tailscale process memory/CPU impact.
- Boot time, uptime over multiple days, heat and power consumption.
- Resource impact of any additional container beyond Navidrome.

Use the following checks after adding any service:

```bash
free -h
df -h /
sudo docker stats --no-stream
sudo docker system df
du -sh ~/soft-and-wet/music ~/soft-and-wet/data/navidrome
```
