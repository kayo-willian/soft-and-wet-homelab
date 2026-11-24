# Operations and maintenance

[Português (Brasil)](pt-BR/operacoes.md) · [Documentation](README.md) · [Home](../README.md)

## Access

From Ubuntu, replace the example values with the Debian account and LAN address.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
ssh "${DEBIAN_USER}@${DEBIAN_HOST}"
```

The current machine hostname is `debian`. Local hostnames resolve only when the network provides suitable DNS or mDNS configuration; the explicit LAN IP is the reliable fallback.

## Container lifecycle

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker compose restart navidrome
sudo docker stats --no-stream
```

`docker compose down` would stop and remove the Compose-created container and network; bind-mounted music and data remain on the host. Do not run cleanup commands against persistent data without an independent backup.

## Updating an application (not yet performed in this project)

Before upgrading, back up application data and review the image release. For a controlled update:

```bash
cd ~/soft-and-wet
sudo docker compose pull navidrome
sudo docker compose up -d navidrome
sudo docker compose ps
```

The initial file uses `:latest` because that was how the container was first launched. A later change to a pinned version is possible but should be documented separately rather than retroactively reported as deployed.

## Transfer new music

On Ubuntu, set the example account and documentation-only IP below to your Debian login and LAN address. The remote path is relative to that account’s home directory. Both machines need `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
```

On Debian, inspect usage:

```bash
du -sh ~/soft-and-wet/music
free -h
df -h /
```

## Backup plan — not yet implemented

The single eMMC is a single point of failure. A safe future workflow is to **stop writes to Navidrome, copy the `data/navidrome/` directory and the music collection to another trusted machine, then test a restoration**. A copy to another directory on the same eMMC is not an independent backup.

The exact tools, schedule, destination and restore verification are not recorded as completed. No `backup.sh` is shipped or represented as working.

## Useful diagnostics

```bash
cd ~/soft-and-wet
sudo systemctl status ssh docker tailscaled
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
tailscale status
sudo docker system df
sudo journalctl -u docker -n 50 --no-pager
```

For each new service, record resource use, ports, persistent paths, logs and the recovery procedure.
