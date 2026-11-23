# Troubleshooting and encountered issues

## 1. `swapon` not in the unprivileged user's PATH

A direct `swapon` call failed. Invoking the utility by its full path worked:

```bash
sudo /usr/sbin/swapon --show
```

It reported `/dev/mmcblk2p3` as a 1.5 GB swap partition with zero use. **No swap resize or zram change was performed.**

## 2. `Unknown Album` in Navidrome

Several downloaded tracks did not contain usable album tags. Navidrome displayed **Unknown Album** in the album view even though names were assigned to the audio files.

MusicBrainz Picard was tried but did not produce the desired result. Rather than falsify album metadata by copying song titles into the album field, the user opened the **Songs** view and kept the library as it was.

## 3. Navidrome missing a transferred track

Check whether the file was actually copied to `~/soft-and-wet/music/Brasil/`, readable by the configured UID/GID and supported by the player. Wait for the configured hourly scan or initiate a scan and inspect logs:

```bash
sudo docker compose logs --tail=100 navidrome
ls -lh ~/soft-and-wet/music/Brasil/
```

## 4. Service accessible on LAN but not on Android

Check that both devices are connected to the same tailnet and use the Debian **Tailscale** IP in the Android client's URL, not the LAN IP when away from home:

```bash
tailscale status
tailscale ip -4
sudo docker compose ps
```

Do not fix access problems by exposing 4533 directly to the internet.

## 5. Limited RAM and eMMC storage

Monitor the host before adding new services:

```bash
free -h
df -h /
sudo docker stats --no-stream
sudo docker system df
```

The low baseline usage was encouraging, but a single startup sample cannot predict resource usage of several simultaneous containers.

## 6. `rsync` duplicates after renaming

Repeating the same `rsync` command updates files with unchanged paths. If filenames are changed locally, old copies may remain at the destination. Review source and destination before any cleanup. Avoid adding `--delete` without confirming what it would remove.
