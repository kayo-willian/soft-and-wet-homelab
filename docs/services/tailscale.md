# Tailscale — deployed host service

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

**Private network addresses are intentionally omitted from this public repository.** Store them in local notes that are excluded from Git.

## Mobile configuration

1. Install the Tailscale Android app and sign in to the same tailnet.
2. Turn on the VPN connection on the phone.
3. Open a compatible Navidrome/Subsonic music client on Android.
4. Configure its server URL as `http://<DEBIAN_TAILSCALE_IP>:4533` with the Navidrome account credentials.
5. Test access outside the home Wi-Fi, with Tailscale connected.

The user confirmed this setup worked. The exact music client installed on Android was not conclusively recorded, so it is not identified as a fact here.

## Operational commands

```bash
tailscale status
tailscale ip -4
sudo systemctl status tailscaled
```

If mobile access stops working, verify both devices are online in Tailscale, Navidrome is running, and port 4533 is reachable on the expected host interface. Never expose this port by creating a router port-forward merely to fix Tailscale connectivity.

## Security note

The Tailscale tunnel protects traffic between tailnet devices, but Navidrome is also currently published by Docker on all host interfaces. Private remote access **does not automatically restrict local-network exposure**; this needs a separate review.

Official docs: https://tailscale.com/docs/install/linux
