# Biblioteca musical — Brasil

[English](../music-library.md) · [Documentação](README.md) · [Início](../../README.pt-BR.md)

## Primeira seleção

A primeira coleção contém **dez faixas** escolhidas manualmente de uma playlist pessoal do Spotify chamada **Brasil**. A seleção mistura MPB, samba, bossa nova e outros estilos. O Spotify **não foi a origem dos arquivos transferidos para o Navidrome**: o servidor utiliza arquivos locais. Não houve registro verificável da origem/licenciamento individual de cada download.

| Nº | Música | Artista(s) |
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

A tabela é o catálogo da **ordem escolhida**, não uma prova de álbum, lançamento, bitrate ou formato. Como várias faixas mostraram `Unknown Album`, não foram inventados nomes de álbuns ou anos.

## Organização dos arquivos

Origem no Ubuntu: `~/Músicas/Brasil/`. Destino no Debian: `~/soft-and-wet/music/Brasil/`.

Convenção de nomes sugerida:

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

As extensões são **exemplos**: manter o formato real de cada arquivo. Não houve auditoria dos nomes finais em disco.

## Como reproduzir a transferência

No Ubuntu, substitua a conta e o IP de exemplo abaixo pelo login e pelo endereço do Debian na LAN. O caminho remoto parte da pasta pessoal dessa conta. As duas máquinas precisam de `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
```

O `rsync` mantém a origem no Ubuntu e atualiza arquivos no mesmo caminho no Debian. Se forem renomeados no Ubuntu, as cópias com nomes antigos podem permanecer no Debian; conferir antes de remover.

## Apresentação no Navidrome

A tela de álbuns usa o campo **album** dos metadados internos, independentemente do nome do arquivo MP3, da pasta ou da playlist. Algumas músicas mostraram `Unknown Album`. Uma tentativa com MusicBrainz Picard não resolveu a apresentação; foi decidido usar a seção **Músicas (Songs)**, mantendo os metadados de álbum não verificados. Uma playlist com o nome Brasil dentro do Navidrome é independente da pasta.

## Publicação

Somente o catálogo de texto é público. Não adicionar áudio, capas, credenciais do Spotify, bancos de usuários, cache do Navidrome ou mídia bruta.
