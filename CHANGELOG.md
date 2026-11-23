# Changelog

This file records actual setup milestones. Future services are not entered as completed.

## 2026-11-23 — First working service

- Confirmed the Debian 13 headless server was reachable via SSH from another room.
- Verified Docker and Docker Compose; `docker compose version` reported `v5.8.0`.
- Measured baseline RAM, swap and storage usage before running application containers.
- Created `~/soft-and-wet`, the Navidrome Compose file, `.env` containing local UID/GID, and `.gitignore`.
- Pulled `navidrome/navidrome:latest`, created the Compose network and started the `navidrome` container.
- Opened the Navidrome first-run administrator page on port 4533.
- Added ten selected tracks from the **Brasil** music collection; viewed them in Navidrome.
- Encountered `Unknown Album` for tracks with incomplete album tags; chose the Songs view instead of rewriting album metadata solely for the display.
- Installed and authenticated Tailscale on Debian and Android; reported successful remote mobile access to Navidrome.

## Not yet completed

- Portainer, Heimdall, Uptime Kuma, Pi-hole, Apache or other application containers.
- Documented backup-and-restore test.
- Confirmed SSH-key-only authentication or a restricted Docker port binding.
- Recorded post-library steady-state system resource measurements.
