# Navidrome — deployed service

[Português (Brasil)](../pt-BR/servicos/navidrome.md) · [Documentation](../README.md) · [Home](../../README.md)

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
| `./music` | `/music` | Read-only music library |

The project directory and music files must be readable by the configured user, and the data directory must be writable. Music stays outside Git.

## First-run observations

1. `sudo docker compose up -d` pulled the image and started the container.
2. `sudo docker compose ps` reported `navidrome` running and published port `4533/tcp` on `0.0.0.0` and `[::]`.
3. The browser displayed the initial **Create Admin** page on the LAN address and port 4533.
4. The administrator was set up and ten tracks were transferred to the server and displayed in the library.
5. Some tracks had missing album tags and appeared under **Unknown Album**. The Songs view showed the titles correctly; album metadata was not altered just to change the tile label.
6. Mobile access through the private Tailscale connection subsequently worked.

The administrator credentials are private. I confirmed mobile playback separately from the terminal record and did not record the client name.

## File transfer from Ubuntu

On Ubuntu, set the example account and documentation-only IP below to your Debian login and LAN address. The remote path is relative to that account’s home directory. Both machines need `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
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

The `-v` option to `docker compose down` can remove project-managed named volumes and attached anonymous volumes. This configuration stores the library and database in host directories (bind mounts).

## Status

**Deployed and tested.** The lab has one application container and a small library. Long-term measurements, an independent backup and restore test, and restricted port bindings are still pending.

## Official references

- [Navidrome](https://www.navidrome.org/)
- [Docker installation](https://www.navidrome.org/docs/installation/docker/)
