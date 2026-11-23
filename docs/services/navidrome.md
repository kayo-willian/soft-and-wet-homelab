# Navidrome — deployed service

## Why it is here

Navidrome is the first useful application in the lab: it indexes locally stored music and makes it available via browser and a compatible Android client. The server does not download tracks automatically. A small curated library avoids uncontrolled storage growth.

## Exact initial deployment

The first application container was created with the following configuration. This is a copy of the top-level [`compose.yaml`](../../compose.yaml), preserved to document the deployed state:

```yaml
services:
  navidrome:
    image: navidrome/navidrome:latest
    container_name: navidrome
    user: "${HOST_UID}:${HOST_GID}"
    ports:
      - "4533:4533"
    restart: unless-stopped
    volumes:
      - ./data/navidrome:/data
      - ./music:/music:ro
    environment:
      ND_LOGLEVEL: info
      ND_SCANSCHEDULE: 1h
    logging:
      driver: json-file
      options:
        max-size: "5m"
        max-file: "2"
```

The `.env` contains `HOST_UID` and `HOST_GID`, populated using `id -u` and `id -g`. This keeps the application data owned by a regular local user rather than relying on root-owned application files.

## Volumes and permissions

| Host path | Container path | Purpose |
| --- | --- | --- |
| `./data/navidrome` | `/data` | Writable database and cache |
| `./music` | `/music:ro` | Read-only music library |

The project directory and music files must be readable by the configured user, and the data directory must be writable. Music stays outside Git.

## First-run observations

1. `sudo docker compose up -d` pulled the image and started the container.
2. `sudo docker compose ps` reported `navidrome` running and published port `4533/tcp` on `0.0.0.0` and `[::]`.
3. The browser displayed the initial **Create Admin** page on the LAN address and port 4533.
4. The administrator was set up and ten tracks were transferred to the server and displayed in the library.
5. Some tracks had missing album tags and appeared under **Unknown Album**. The Songs view showed the titles correctly; album metadata was not altered just to change the tile label.
6. Mobile access through the private Tailscale connection subsequently worked.

> The user's specific administrator credentials are not documented or published. The precise Android music-client application was not independently recorded in a terminal log.

## File transfer from Ubuntu

Files were organized in the Ubuntu folder `~/Músicas/Brasil/` and transferred over SSH to Debian:

```bash
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  kayo@<DEBIAN_LAN_IP>:/home/kayo/soft-and-wet/music/Brasil/
```

The server recognizes the files during its next scan (`ND_SCANSCHEDULE=1h`). A manual scan can be initiated from the Navidrome UI, if available in the installed version. Check index status and logs rather than assuming the scan succeeded.

## Useful operations

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker stats --no-stream navidrome
du -sh ~/soft-and-wet/music ~/soft-and-wet/data/navidrome
df -h /
```

Do not use `docker compose down -v` without understanding what it will delete. The bind-mounted data lives under the host directory and is not meant for GitHub.

## Status

**Deployed and tested.** A large music library, full backup/recovery test, and hardened network binding have **not** been completed.

Official docs: https://www.navidrome.org/docs/installation/docker/
