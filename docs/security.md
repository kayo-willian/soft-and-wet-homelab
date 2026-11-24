# Security and publication boundaries

[Português (Brasil)](pt-BR/seguranca.md) · [Documentation](README.md) · [Home](../README.md)

## Implemented or directly observed

- The server is headless and administered remotely over SSH.
- The Navidrome container uses a host UID/GID supplied through an untracked `.env` file.
- `/music` is mounted read-only inside the container.
- The [ignore rules](../.gitignore) cover local media and data directories, `.env`, key files and logs. Review staged files for sensitive content as well.
- Tailscale connects Debian and the Android device through a private tailnet.
- No public router port-forward was required for remote music playback.

## Not verified or not yet done

- SSH key-only authentication and disabling password login.
- Active UFW rule set, Fail2ban jail status or Docker-specific firewall policy.
- Restricting Navidrome's published port to specific interfaces.
- TLS directly on Navidrome's local HTTP endpoint.
- Formal backup and restore test.
- Pinned immutable Navidrome image digest and automated vulnerability monitoring.

The [Compose file](../compose.yaml) publishes TCP 4533 on all host interfaces. Tailscale encrypts remote traffic. The published port can still be reachable from the LAN. Docker can forward traffic to a container before the usual UFW input rules apply. Interface restrictions and Docker firewall rules still need a separate test.

## Before publishing manually

```bash
git status --short
git diff --cached --name-only
```

Do not publish: `.env`, `data/`, `music/`, `secrets/`, `local/`, `backups/`, private IP/device lists, credentials, private keys, Tailscale account details, API tokens, database files, audio files or raw logs. **GitHub's browser upload interface may not protect you in the same way as a local `git add` honoring `.gitignore`; select files deliberately.**

This personal lab has no formally validated production security controls. For this single-host lab, keep repository docs free of usernames/passwords and avoid storing unnecessary details about the home network.
