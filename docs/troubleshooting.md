# Troubleshooting, alternatives and decisions

[Português (Brasil)](pt-BR/solucao-de-problemas.md) · [Documentation](README.md) · [Home](../README.md)

This log distinguishes **encountered problems**, **discussed alternatives**, **successful checks**, and **open risks**. Terminal evidence exists only for some steps.

## A. Problems actually encountered

### T01 — `swapon`: command not found

- **Observed:** `swapon` was not available by its short name in the regular user's shell.
- **Attempt / fix:** invoked `sudo /usr/sbin/swapon --show`, which worked.
- **Result:** `/dev/mmcblk2p3` is a **1.5 GiB swap partition**, 0 used at the time.
- **Alternatives considered:** increasing total swap to 4 GB, adding a 2.5 GB swapfile, zram.
- **Decision:** **no partition resize, swapfile or zram was applied**; measure workload first.

```bash
sudo /usr/sbin/swapon --show
cat /proc/swaps
free -h
```

### T02 — Navidrome shows `Unknown Album`

- **Observed:** Several imported tracks showed album artwork and artist/track information but the album tile read `Unknown Album`.
- **Cause:** missing or unsuitable embedded `album` tags. Renaming an MP3/FLAC file alone does not populate the tag.
- **Attempted:** MusicBrainz Picard, with discussion of Scan, metadata updates and copying modified files with `rsync`.
- **Result:** Picard **did not solve the display issue** in the observed session.
- **Alternative:** populate album tags with each song name. **Rejected** because it would misrepresent songs as albums.
- **Chosen solution:** use Navidrome's **Songs** view, which displays track titles; leave unverified albums alone.

### T03 — Finding a Navidrome mobile app

- **Observed:** there is no official Navidrome-branded Android APK in the main Navidrome releases.
- **Options considered:** third-party Subsonic/OpenSubsonic clients, including Symfonium, Substreamer, Ultrasonic, Navis and Chora.
- **Result:** I later confirmed that a compatible app worked on the Samsung A34, including remote playback.
- **Evidence limit:** the exact installed third-party client was **not conclusively recorded**, so no specific app is claimed as deployed.
- **Correct configuration:** use the Debian tailnet IP on port 4533, not the LAN IP when outside home Wi-Fi.

### T04 — Spotify playlists versus locally owned files

- **Observed need:** replicate a personal Spotify playlist in Navidrome.
- **Constraint:** Spotify Premium offline cache is not ordinary transferable MP3/FLAC.
- **Options discussed:** buy/download legally available audio, use compatible audio-download tools for permitted sources, curate a separate collection.
- **Decision:** manually select ten tracks from a playlist named **Brasil**, obtain local files, and transfer them to Debian. Download source and license provenance were not audited; no files are published.

### T05 — Folder versus album versus playlist

- **Observed:** folder `music/Brasil/` contains the ten selected tracks; Navidrome album view does not automatically treat this directory as one album.
- **Explanation:** the media library primarily indexes embedded music tags. Folder naming does not determine the album tag.
- **Decision:** view tracks in **Songs**. A named Navidrome playlist can be created separately; no playlist export or automation was recorded.

## B. Design alternatives explored (not implementation failures)

| Topic | Option considered | Decision | Why |
| --- | --- | --- | --- |
| Storage | Ventoy USB drive (~51 GB free) | **Excluded** from permanent setup | I wanted to keep removable media unplugged |
| Storage | Internal eMMC | **Used** | Simple, but small and a single point of failure |
| Swap | Resize 1.5 GiB swap to 4 GB | **Not done** | No observed memory pressure; partition modification risk |
| Swap | Additional swapfile or zram | **Not done** | Reconsider after real load testing |
| Web server | Apache installed via Debian tasksel | **Not installed** | No current use; optional future container exercise |
| Media | Plex/Jellyfin | **Deferred** | Music-only focus, limited resources |
| Downloads | Sonarr/Radarr and movie automation | **Excluded** | Unnecessary storage consumption |
| Remote access | Router port-forwarding | **Avoided** | Tailscale worked privately |
| Remote access | Tailscale native host daemon | **Implemented** | Android remote streaming confirmed |
| Docker management | Portainer | **Not installed** | Next possible service; not part of current stack |
| Monitoring | Uptime Kuma/Beszel | **Not installed** | Resource assessment first |
| Logs | Dozzle | **Not installed** | Optional convenience |
| Dashboard | Heimdall | **Not installed** | Optional convenience |
| File manager | File Browser | **Not installed** | Original project archived |
| Backup | Copy to another independent computer | **Not done** | Plan only; no restore test |
| Power | Ignore laptop lid close in logind | **Suggested, unverified** | Actual persistent setting not verified |

## C. Repeatable diagnostics

### Navidrome container not reachable

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker stats --no-stream
```

Check the web UI at `http://<DEBIAN_LAN_IP>:4533` locally. If remote, use `http://<DEBIAN_TAILSCALE_IP>:4533`; do not forward the router's TCP 4533.

### Imported music not shown

```bash
cd ~/soft-and-wet
ls -lh music/Brasil/
du -sh music
sudo docker compose logs --tail=100 navidrome
```

Verify the audio files are readable by the configured UID/GID, supported, and scanned. The installed Compose configured `ND_SCANSCHEDULE: 1h`.

### Duplicate tracks after `rsync`

`rsync -avh` updates matching paths, but it **does not delete** old filenames if files are renamed or moved. Review the remote folder before deleting anything; avoid a blind `--delete`.

### Low storage or high memory pressure

```bash
df -h /
free -h
sudo docker system df
sudo docker stats --no-stream
sudo /usr/sbin/swapon --show
```

The original 32.51 MiB Navidrome reading was a **startup sample**, not stable consumption under a large library.

### SSH or Tailscale unavailable

```bash
sudo systemctl status ssh docker tailscaled --no-pager
tailscale status
tailscale ip -4
hostname -I
```

Do not assume name resolution for `debian` works on every LAN. Use the actual address when necessary.

## D. Open technical risks, not yet resolved

1. Navidrome is published on `0.0.0.0:4533` and `[::]:4533`; Tailscale does not restrict this by itself.
2. There is no confirmed backup-and-restore exercise for Navidrome state and music.
3. There is no measured steady-state RAM, CPU or free-disk result after adding the ten tracks.
4. SSH key-only access, UFW/Fail2ban configuration, Docker firewall behavior and image version pinning were not verified.
5. No automated scripts or additional application containers are actually deployed.

See [security](security.md) and [operations](operations.md). Do not represent suggested mitigation commands as completed changes.
