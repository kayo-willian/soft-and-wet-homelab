# Operations and maintenance

## Access

Use SSH from the Ubuntu computer to administer Debian:

```bash
ssh kayo@<DEBIAN_LAN_IP>
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

On Ubuntu:

```bash
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  kayo@<DEBIAN_LAN_IP>:/home/kayo/soft-and-wet/music/Brasil/
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
sudo systemctl status ssh docker tailscaled
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
tailscale status
sudo docker system df
journalctl -u docker -n 50 --no-pager
```
