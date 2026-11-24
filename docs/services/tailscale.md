# Tailscale — deployed host service

[Português (Brasil)](../pt-BR/servicos/tailscale.md) · [Documentation](../README.md) · [Home](../../README.md)

## Purpose

Tailscale provides a private network connection between the Debian home server and a Samsung A34 Android phone, allowing Navidrome to be reached without port-forwarding it on the home router. Tailscale runs directly on **Debian**, not as a Docker container.

## Installation path used

The project followed these official Linux installation instructions:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
tailscale ip -4
```

`tailscale up` presented an authentication URL. After both devices were authorized under the same tailnet, Tailscale assigned each one an address in its private address space.

**Private network addresses are intentionally omitted from this public repository.** Store addresses in local notes outside Git and keep account details and authentication tokens private.

## Mobile configuration

1. Install the Tailscale Android app and sign in to the same tailnet.
2. Turn on the VPN connection on the phone.
3. Open a compatible Navidrome/Subsonic music client on Android.
4. Configure its server URL as `http://<DEBIAN_TAILSCALE_IP>:4533` with the Navidrome account credentials.
5. Turn off home Wi-Fi and test playback over mobile data with Tailscale connected.

I later confirmed remote playback on the Samsung A34. I did not record the name of the music client.

## Operational commands

```bash
tailscale status
tailscale ip -4
sudo systemctl status tailscaled
```

If mobile access stops working, verify both devices are online in Tailscale, Navidrome is running, and port 4533 is reachable on the expected host interface. Never expose this port by creating a router port-forward merely to fix Tailscale connectivity.

## Security note

The Tailscale tunnel protects traffic between tailnet devices, but Navidrome is also currently published by Docker on all host interfaces (`0.0.0.0:4533` and `[::]:4533`). Private remote access **does not automatically restrict local-network exposure**; this needs a separate review.

## Official references

- [Tailscale](https://tailscale.com/)
- [Linux installation](https://tailscale.com/docs/install/linux)
