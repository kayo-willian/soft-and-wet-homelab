# Arquitetura

[English](../architecture.md) · [Índice](README.md)

## Objetivo

O Soft & Wet é um homelab self-hosted com orçamento rígido de recursos. Funciona em um notebook Positivo com Intel Atom x5-Z8350, 2 GB físicos de RAM e armazenamento interno eMMC limitado. Usa Debian 13 sem interface gráfica, administrado remotamente por SSH.

## Topologia implementada

~~~text
Ubuntu (PC principal) ─────────────── rede local ─────────────┐
       SSH / origem das músicas                               │
                                                               v
                                                Positivo — Debian 13
                                                 hostname: debian
                                                 - OpenSSH (host)
                                                 - Docker Engine
                                                 - Tailscale (host)
                                                       |
                                                 Docker Compose
                                                       |
                                            soft-and-wet_default
                                                       |
                                                 Navidrome :4533
                                                 /data  leitura/escrita
                                                 /music somente leitura
                                                       ^
                                                       |
                                                  eMMC interno

                                         Rede privada do Tailscale
                                   Debian <================> Android
                                             túnel criptografado
                                                  player musical
~~~

## Armazenamento

Sistema operacional, dados do Docker, banco/cache do Navidrome e biblioteca musical ficam no **eMMC interno**. A ideia inicial de usar permanentemente o pendrive Ventoy foi **descartada**.

| Diretório no Debian | Conteúdo | Publicado? |
|---|---|---|
| `~/soft-and-wet/compose.yaml` | Definição da aplicação | Sim, versão reproduzível |
| `~/soft-and-wet/.env` | UID/GID locais | Não |
| `~/soft-and-wet/data/navidrome/` | Banco e cache | Não |
| `~/soft-and-wet/music/Brasil/` | Dez músicas | Não |

O repositório GitHub se chama `soft-and-wet-homelab`; a pasta real no Debian é `~/soft-and-wet`. São nomes intencionalmente diferentes.

## Contêineres e rede

Há **um único contêiner de aplicação**, `navidrome`, na rede `soft-and-wet_default`. A porta TCP 4533 foi publicada no host. **Tailscale não é contêiner**: está instalado como serviço do Debian.

## Limites

- Host headless, sem desktop gráfico.
- Sem VM, sem disco externo permanente e sem automação de filmes.
- Cada novo serviço exige medição de RAM e armazenamento.
- Sem alta disponibilidade ou comprovação de backup externo.
- A publicação atual da porta 4533 em todas as interfaces ainda precisa ser revisada.

Veja [medições](measurements.md), [segurança](security.md) e [rede](networking.md).
