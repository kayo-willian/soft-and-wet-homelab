# Changelog — Soft & Wet

[Português (Brasil)](HISTORICO.pt-BR.md) · [Documentation](docs/README.md) · [Home](README.md)

This file records actual setup milestones. Future services are not entered as completed.

## 2026-11-23 — First working service

- Confirmed the Debian 13 headless server was reachable via SSH from another room.
- Verified Docker and Docker Compose; `docker compose version` reported `v5.8.0`.
- Measured baseline RAM, swap and storage usage before running application containers.
- Created `~/soft-and-wet`, the Navidrome Compose file, `.env` containing local UID/GID, and `.gitignore`.
- Pulled `navidrome/navidrome:latest`, created the `soft-and-wet_default` network and started the `navidrome` container.
- Opened the first-run page on TCP 4533 and set up the Navidrome administrator account.
- Added ten selected tracks from the **Brasil** collection to its own directory and viewed them in Navidrome.
- Encountered `Unknown Album` for tracks with incomplete album tags; tried MusicBrainz Picard and chose the Songs view instead of rewriting album metadata solely for the display.
- Installed and authenticated Tailscale on Debian and the Samsung A34. I later confirmed remote playback on the phone.

## Not yet completed

- Portainer, Heimdall, Uptime Kuma, Pi-hole, Apache or other application containers.
- Independent backup and documented restore test.
- Confirmed SSH-key-only authentication or a restricted Docker port binding.
- Recorded post-library steady-state system resource measurements.
- Custom automation scripts.

See [installation](docs/installation.md), [troubleshooting](docs/troubleshooting.md) and the [roadmap](docs/roadmap.md).

## Documentation changes

- Localized Portuguese filenames and linked each page to its translation, documentation index and project README.
