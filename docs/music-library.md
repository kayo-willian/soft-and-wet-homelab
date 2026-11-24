# Music library — Brasil

[Português (Brasil)](pt-BR/biblioteca-musical.md) · [Documentation](README.md) · [Home](../README.md)

## Initial selection

The initial collection contains ten tracks selected manually from a Spotify playlist named **Brasil**. The playlist mixes Brazilian popular music, samba, bossa nova and related styles. Spotify itself is **not** the file source for Navidrome; the server indexes music files transferred to Debian. The specific download provenance and licensing of individual files were not recorded.

| # | Title | Artist(s) |
| ---: | --- | --- |
| 01 | Carolina, Carol Bela | Toquinho; Jorge Ben Jor |
| 02 | Onde Anda Você | Toquinho; Vinicius de Moraes |
| 03 | Meu Rio | Toco |
| 04 | Deixa pra Lá | Bossacucanova; Teresa Cristina |
| 05 | Amiga da Minha Mulher | Seu Jorge |
| 06 | Disritmia | Casuarina |
| 07 | Garota de Ipanema | Tom Jobim |
| 08 | Chove Chuva | Jorge Ben Jor |
| 09 | Bis | Arthur Verocai; Azymuth |
| 10 | Corre pra Ver | Joca Perpignan; Denise Reis |

This table is a catalog of the **selection order**, not proof of an album, release year, bitrate or audio codec. The original metadata for several files displayed **Unknown Album**; the correct album edition was not verified, so album names and years are intentionally omitted.

## File organization

The Ubuntu source folder was `~/Músicas/Brasil/`. The target folder on Debian was `~/soft-and-wet/music/Brasil/`.

A numbered filename convention was recommended, for example:

```text
01 - Carolina, Carol Bela - Toquinho & Jorge Ben Jor.mp3
02 - Onde Anda Você - Toquinho & Vinicius de Moraes.mp3
03 - Meu Rio - Toco.mp3
04 - Deixa pra Lá - Bossacucanova & Teresa Cristina.mp3
05 - Amiga da Minha Mulher - Seu Jorge.mp3
06 - Disritmia - Casuarina.mp3
07 - Garota de Ipanema - Tom Jobim.mp3
08 - Chove Chuva - Jorge Ben Jor.mp3
09 - Bis - Arthur Verocai & Azymuth.mp3
10 - Corre pra Ver - Joca Perpignan & Denise Reis.mp3
```

The extensions here are **examples**; keep each actual downloaded file's original format. The exact final disk filenames have not been audited.

## Reproducing the transfer

On Ubuntu, set the example account and documentation-only IP below to your Debian login and LAN address. The remote path is relative to that account’s home directory. Both machines need `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
```

`rsync` leaves source files on Ubuntu. Repeating this command updates files at the same path; files renamed or moved at the source may leave old copies on the server unless the destination is reviewed separately.

## Navidrome display

Navidrome's album view uses embedded **album tags**, not the MP3 filename or playlist name. Missing album tags produced **Unknown Album**. A Picard metadata attempt did not solve the display issue; the decision was to keep the music and use the **Songs** view. A real Navidrome playlist named Brasil can be curated independently from album tags.

## Publication boundary

Only this text catalog is public. Do not add audio files, album-art downloads, Spotify credentials, user databases, or raw media to Git.
