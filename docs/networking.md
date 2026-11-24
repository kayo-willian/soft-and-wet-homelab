# Networking

[Português (Brasil)](pt-BR/redes.md) · [Documentation](README.md) · [Home](../README.md)

## Confirmed layout

The Debian host is on the same home LAN as the Ubuntu desktop. SSH is available for remote administration, and Navidrome can be reached in the browser at `http://<DEBIAN_LAN_IP>:4533`.

The phone accesses the Debian host via Tailscale using `http://<DEBIAN_TAILSCALE_IP>:4533` while the phone is connected to the tailnet. Real LAN and tailnet IP addresses are intentionally excluded from the public documentation.

## Observed interfaces and bridges

The recorded interface list included the LAN address, IPv6 and Docker bridges for `docker0` and the Compose network `soft-and-wet_default`.

The initial output of `docker compose ps` included:

```text
navidrome ... Up ... 0.0.0.0:4533->4533/tcp, [::]:4533->4533/tcp
```

This means Docker published port 4533 on all IPv4 and IPv6 host interfaces, not only Tailscale. It does **not** by itself prove that the port is reachable from the public internet; router/NAT/firewall conditions matter. No router port-forward was configured as part of this lab.

## Protocols and roles

| Protocol | Used for |
| --- | --- |
| SSH | Headless administration and `rsync` file transfer |
| HTTP/TCP 4533 | Navidrome browser and client access |
| Tailscale | Private encrypted connectivity for remote mobile access |
| Docker bridge | Container network isolation and host port publishing |

## Checks

```bash
cd ~/soft-and-wet
hostname -I
ip -br address
sudo docker network ls
sudo docker compose ps
tailscale status
tailscale ip -4
```

## Caution

Docker-published ports may bypass ordinary UFW traffic filtering. Do not assume that installing UFW alone restricts this container. A dedicated review of host-interface binding and Docker firewall policy is still outstanding.

## Official references

- [Docker port publishing](https://docs.docker.com/engine/network/port-publishing/)
- [Tailscale IP and DNS addresses](https://tailscale.com/docs/concepts/ip-and-dns-addresses)
