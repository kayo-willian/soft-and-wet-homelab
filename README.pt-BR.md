<div align="center">

![Capa Soft & Wet](assets/soft-and-wet-cover.svg)

# Soft & Wet
### Debian 13 · Homelab pessoal

[![English](https://img.shields.io/badge/READ%20IN-ENGLISH-0D0D0D?style=for-the-badge&labelColor=2F81F7)](README.md)
[![Status](https://img.shields.io/badge/STATUS-LAB%20ATIVO-0D0D0D?style=for-the-badge&labelColor=2F81F7)](CHANGELOG.pt-BR.md)
[![Debian](https://img.shields.io/badge/DEBIAN-13-0D0D0D?style=for-the-badge&logo=debian&logoColor=2F81F7)](https://www.debian.org/)
[![Docker](https://img.shields.io/badge/DOCKER-COMPOSE-0D0D0D?style=for-the-badge&logo=docker&logoColor=2F81F7)](https://docs.docker.com/compose/)
[![Tailscale](https://img.shields.io/badge/ACESSO-SSH%20%2B%20TAILSCALE-0D0D0D?style=for-the-badge&logo=tailscale&logoColor=2F81F7)](https://tailscale.com/)
[![RAM](https://img.shields.io/badge/RAM-2%20GB-0D0D0D?style=for-the-badge&labelColor=2F81F7)](docs/pt-BR/measurements.md)
[![MIT](https://img.shields.io/badge/LICEN%C3%87A-MIT-0D0D0D?style=for-the-badge&labelColor=2F81F7)](LICENSE)

**Projeto pessoal de infraestrutura em um notebook físico limitado — não uma ferramenta pronta para distribuição.**

[Arquitetura](docs/pt-BR/architecture.md) · [Instalação](docs/pt-BR/installation.md) · [Serviços](docs/pt-BR/services/README.md) · [Solução de problemas](docs/pt-BR/troubleshooting.md) · [Documentação completa](docs/pt-BR/README.md)

</div>

---

## Por que fiz esse projeto

Quero administrar um servidor real em outro cômodo, testar soluções, investigar falhas e evoluir com o hardware que já possuo. O Soft & Wet é minha primeira infraestrutura self-hosted. Utilizo Debian, SSH, Docker, redes e acesso remoto privado, registrando **o que funcionou, o que não funcionou e o que ainda não foi feito**.

O nome vem do Stand **Soft & Wet**, de *JoJolion*. A capa original do projeto usa uma identidade visual inspirada em mangá, com bolhas e elementos da infraestrutura.

## Implementação atual

| Componente | Como foi implantado | Verificação |
|---|---|---|
| **Debian 13 / OpenSSH** | Positivo sem interface gráfica | Administração via SSH pelo Ubuntu |
| **Docker Engine + Compose** | No Debian | Compose v5.8.0; primeiro contêiner iniciado |
| **Navidrome** | Um contêiner Docker, porta 4533 | Interface web e biblioteca de dez músicas |
| **Tailscale** | Instalado no Debian e Android | Streaming remoto confirmado |
| **Biblioteca Brasil** | Dez músicas escolhidas manualmente no eMMC | Exibidas na seção Músicas |
| Portainer / Uptime Kuma / Heimdall | Não instalados | Avaliação futura conforme recursos |

<div align="center">

### Serviços e sites oficiais

[![Navidrome](https://img.shields.io/badge/NAVIDROME-M%C3%9ASICAS-0D0D0D?style=for-the-badge&labelColor=2F81F7)](https://www.navidrome.org/)
[![Tailscale](https://img.shields.io/badge/TAILSCALE-REDE%20PRIVADA-0D0D0D?style=for-the-badge&logo=tailscale&logoColor=2F81F7)](https://tailscale.com/)
[![Portainer](https://img.shields.io/badge/PORTAINER-PLANEJADO-0D0D0D?style=for-the-badge&logo=portainer&logoColor=8B99AB)](https://www.portainer.io/)
[![Uptime Kuma](https://img.shields.io/badge/UPTIME%20KUMA-PLANEJADO-0D0D0D?style=for-the-badge&logo=uptimekuma&logoColor=8B99AB)](https://github.com/louislam/uptime-kuma)
[![Heimdall](https://img.shields.io/badge/HEIMDALL-PLANEJADO-0D0D0D?style=for-the-badge&logo=linux&logoColor=8B99AB)](https://heimdall.site/)

</div>

## Ambiente físico

| Item | Especificação |
|---|---|
| Máquina | Notebook Positivo |
| CPU | Intel Atom x5-Z8350 |
| RAM | 2 GB físicos (~1,8 GiB visíveis ao Linux) |
| Armazenamento | Apenas eMMC interno; partição raiz de 27G |
| Sistema | Debian 13 (Trixie), sem interface gráfica |
| Hostname | `debian` |
| Administração | SSH a partir do Ubuntu |
| Contêineres | Docker Engine e Docker Compose |
| Cliente remoto | Samsung A34, Tailscale e aplicativo compatível |

A ideia inicial de manter um pendrive Ventoy conectado foi **descartada**. Músicas e dados persistentes ficam no armazenamento interno.

## Arquitetura

~~~text
Ubuntu ── SSH / rsync ── rede local ──┐
                                    │
                               Positivo
                           Debian 13 headless
                             ├── OpenSSH
                             ├── Docker
                             │   └── Compose
                             │       └── Navidrome :4533
                             │           ├── /data  (escrita)
                             │           └── /music (somente leitura)
                             └── Tailscale
                                   ║ rede privada
                                   ╚══ Android + player
~~~

O Navidrome é o **único contêiner de aplicação instalado**. A rede Compose se chama `soft-and-wet_default`. A porta 4533 ainda está publicada em todas as interfaces do host. Isso será revisado; Tailscale não elimina automaticamente a exposição na rede local.

## Reproduzir o primeiro contêiner

Em um Debian 13 já preparado com Docker e Compose:

~~~bash
git clone https://github.com/kayo-willian/soft-and-wet-homelab.git
cd soft-and-wet-homelab
mkdir -p data/navidrome music
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
sudo docker compose config
sudo docker compose up -d
sudo docker compose ps
~~~

Isso recria o serviço Navidrome, não toda a instalação inicial nem a autenticação do Tailscale. Nenhuma música ou banco de dados privado está incluído. Consulte [Instalação](docs/pt-BR/installation.md) e [Tailscale](docs/pt-BR/services/tailscale.md).

## Medições registradas

| Observação | Resultado |
|---|---|
| Partição raiz antes do Navidrome | 27G total / 2,0G usado / 23G disponíveis |
| RAM antes do primeiro contêiner | 388 MiB usados; ~1,4 GiB disponíveis |
| Swap | 1,5 GiB em partição; 0 em uso na coleta |
| RAM do Navidrome na inicialização | 32,51 MiB (uma amostra) |
| CPU do Navidrome na inicialização | 15,83% (instantâneo, **não** média) |

Ainda não há medições estáveis após a biblioteca nem testes com vários serviços. [Medições completas](docs/pt-BR/measurements.md).

## Documentação técnica

A documentação segue uma organização por áreas, inspirada no meu projeto [Enterprise NOC — From Scratch](https://github.com/kayo-willian/enterprise-noc-from-scratch), respeitando a escala menor deste servidor.

| Área | Português | English |
|---|---|---|
| Índice | [Documentação](docs/pt-BR/README.md) | [Index](docs/README.md) |
| Arquitetura | [Arquitetura](docs/pt-BR/architecture.md) | [Architecture](docs/architecture.md) |
| Instalação | [Instalação](docs/pt-BR/installation.md) | [Installation](docs/installation.md) |
| Serviços | [Serviços](docs/pt-BR/services/README.md) | [Services](docs/services/README.md) |
| Rede | [Redes](docs/pt-BR/networking.md) | [Networking](docs/networking.md) |
| Operação | [Operações](docs/pt-BR/operations.md) | [Operations](docs/operations.md) |
| Segurança | [Segurança](docs/pt-BR/security.md) | [Security](docs/security.md) |
| Recursos | [Medições](docs/pt-BR/measurements.md) | [Measurements](docs/measurements.md) |
| Músicas | [Biblioteca Brasil](docs/pt-BR/music-library.md) | [Music library](docs/music-library.md) |
| Problemas | [Solução de problemas](docs/pt-BR/troubleshooting.md) | [Troubleshooting](docs/troubleshooting.md) |
| Expansões | [Roadmap](docs/pt-BR/roadmap.md) | [Roadmap](docs/roadmap.md) |

## Possibilidade de evolução

**Soft & Wet → Go Beyond → Made in Heaven**. São estágios possíveis de aprendizado e infraestrutura, não lançamentos obrigatórios.

~~~text
01 soft-and-wet-homelab       | Positivo · Debian 13       | ATUAL
02 soft-and-wet-plunder       | Positivo · Alpine/OpenRC   | Experimento paralelo
03 soft-and-wet-homelab-v2    | Notebook melhor           | Condicional
04 soft-and-wet-homelab-v3    | Nova arquitetura          | Opcional
05 go-beyond                 | Desktop i7-3770 reciclado | Futuro
06 made-in-heaven            | Infraestrutura maior      | Longo prazo
~~~

Veja o [roadmap detalhado](docs/pt-BR/roadmap.md). Não há datas ou compromisso de executar todas as fases.

## Escopo e limitações

- Um único equipamento físico, sem VM e sem interface gráfica.
- Sem automação de filmes/séries nem pendrive permanente.
- RAM, armazenamento e backups são limitações reais.
- Não afirmo segurança de produção, alta disponibilidade, backups testados nem implantação dos serviços planejados.
- Música, cache, banco do Navidrome, `.env`, senhas e IPs privados não fazem parte do Git.

## Licença

MIT — [LICENSE](LICENSE). A licença cobre os arquivos deste repositório, **não** as músicas nem os programas de terceiros.
