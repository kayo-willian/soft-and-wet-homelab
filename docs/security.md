# Security and publication boundaries

## Implemented or directly observed

- The server is headless and administered remotely over SSH.
- The Navidrome container uses a host UID/GID supplied through an untracked `.env` file.
- `/music` is mounted read-only inside the container.
- Runtime media, application data and secrets are excluded by `.gitignore`.
- Tailscale connects Debian and the Android device through a private tailnet.
- No public router port-forward was required for remote music playback.

## Not verified or not yet done

- SSH key-only authentication and disabling password login.
- Active UFW rule set, Fail2ban jail status or Docker-specific firewall policy.
- Restricting Navidrome's published port to specific interfaces.
- TLS directly on Navidrome's local HTTP endpoint.
- Formal backup and restore test.
- Pinned immutable Navidrome image digest and automated vulnerability monitoring.

**Important:** the current Compose file publishes `4533:4533`, so Navidrome listens on all host interfaces. A working Tailscale connection does not remove local exposure. Docker port publishing can interact unexpectedly with UFW. Treat this as an item for a separate tested hardening change; do not silently present it as already solved.

## Before publishing manually

```bash
# From the directory containing the files to be published:
git status --short
git diff --cached --name-only
```

Do not publish: `.env`, `data/`, `music/`, `secrets/`, `local/`, `backups/`, private IP/device lists, API tokens, database files, audio files or raw logs. **GitHub's browser upload interface may not protect you in the same way as a local `git add` honoring `.gitignore`; select files deliberately.**

For this single-host lab, keep repository docs free of usernames/passwords and avoid storing unnecessary details about the home network.
