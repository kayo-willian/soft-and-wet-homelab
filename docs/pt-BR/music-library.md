# Biblioteca musical — Brasil

[English](../music-library.md) · [Índice](README.md)

## Primeira seleção

A primeira coleção contém **dez faixas** escolhidas manualmente de uma playlist pessoal do Spotify chamada **Brasil**. A seleção mistura MPB, samba, bossa nova e outros estilos. O Spotify **não foi a origem dos arquivos transferidos para o Navidrome**: o servidor utiliza arquivos locais. Não houve registro verificável da origem/licenciamento individual de cada download.

| Nº | Música | Artista(s) |
|---:|---|---|
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

~~~text
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
~~~

As extensões são **exemplos**: manter o formato real de cada arquivo. Não houve auditoria dos nomes finais em disco.

## Transferência realizada

Executar **no Ubuntu**:

~~~bash
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  kayo@<DEBIAN_LAN_IP>:/home/kayo/soft-and-wet/music/Brasil/
~~~

O `rsync` preserva os arquivos originais. Se forem renomeados no Ubuntu, as cópias com nomes antigos podem permanecer no Debian; conferir antes de remover.

## Apresentação no Navidrome

A tela de álbuns usa o campo **album** dos metadados internos, não o nome da pasta ou da playlist. Algumas músicas mostraram `Unknown Album`. Uma tentativa com MusicBrainz Picard não resolveu a apresentação; foi decidido usar a seção **Músicas (Songs)**, mantendo os metadados de álbum não verificados. Uma playlist com o nome Brasil dentro do Navidrome é independente da pasta.

## Publicação

Somente o catálogo de texto é público. Não adicionar áudio, capas extraídas, credenciais do Spotify, banco ou cache do Navidrome.
