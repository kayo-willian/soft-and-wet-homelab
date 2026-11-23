<div align="center">

![Soft & Wet — capa do homelab em estilo mangá](assets/soft-and-wet-cover.svg)

# Soft & Wet

**Um servidor doméstico real construído com um notebook praticamente obsoleto.**

[Português (Brasil)](README.pt-BR.md) · [English](README.md) · [Documentação completa](docs/pt-BR/README.md)

</div>

---

## O experimento

Eu tinha um notebook Positivo com **2 GB de RAM**, processador Intel Atom e aproximadamente **29 GB de eMMC interno**. Sem servidor dedicado, sem NAS, sem recursos para virtualização pesada e sem espaço para deixar mais um computador ocupando minha mesa.

Em vez de descartar o notebook, instalei **Debian 13 sem ambiente gráfico**, coloquei a máquina em outro cômodo e comecei a executar serviços úteis, administrados remotamente pelo Ubuntu e pelo celular.

**O foco do projeto é fazer um hardware extremamente limitado funcionar como servidor de verdade.** Contêineres, música e acesso remoto são experiências para testar essa ideia, não o objetivo final do repositório. Quero conhecer o sistema, medir o custo de cada serviço, resolver problemas, documentar decisões e expandir somente quando os recursos permitirem.

O nome e a identidade visual são inspirados em *JoJolion*. O laboratório começa pequeno, mas sem limitar suas possibilidades futuras.

## Os três dispositivos

| Papel | Equipamento | Especificações principais |
|---|---|---|
| **Servidor — Soft & Wet** | Notebook Positivo | Intel Atom x5-Z8350 · **2 GB RAM** · ~29 GB de eMMC · **Debian 13**, sem interface gráfica |
| **Administração / origem dos arquivos** | Desktop Ubuntu | **Intel Core i7-3770 (4 núcleos / 8 threads)** · **Intel HD Graphics 4000** · ~16 GB RAM · Ubuntu **26.04.1 LTS** · GNOME 50.1 / Wayland |
| **Cliente remoto** | Samsung Galaxy A34 5G | **6 GB RAM / 128 GB armazenamento** · tela Super AMOLED 6,6″ 120 Hz · 5G · bateria de 5.000 mAh |

O desktop **não é o servidor**: ele serve para acessar por SSH, transferir arquivos e administrar os serviços pelo navegador. O Android é o cliente remoto. O [inventário completo](docs/pt-BR/hardware.md) registra versões, kernel, disco e fontes das especificações.

## O que eu já fiz

| Marco | Implementação | Evidência |
|---|---|---|
| Preparei o sistema mínimo | Debian 13 sem desktop | Servidor operando por SSH em outro cômodo |
| Configurei administração remota | OpenSSH e ferramentas Linux | Acesso pelo Ubuntu |
| Adotei contêineres | Docker Engine + Compose | v5.8.0 reportada; um serviço em execução |
| Testei uma aplicação real | Navidrome e dez músicas locais | Biblioteca e interface web funcionando |
| Testei acesso fora de casa | Tailscale no Debian e Android | Streaming remoto confirmado |
| Medi os recursos | `df`, `free`, `swapon`, `docker stats` | 23G livres antes dos contêineres; 388 MiB de RAM usados |

Até aqui, instalei **um único contêiner de aplicação**. **Portainer, Pi-hole, monitoramento, dashboards e outras ferramentas estão no escopo do laboratório, mas isso não significa que já estejam instalados.** O [registro de serviços](docs/pt-BR/services/README.md) distingue cada estado.

## Ecossistema e tecnologias

Estas são as tecnologias **usadas ou consideradas para a evolução do laboratório**. Os badges são links para os sites oficiais, *não indicadores de conclusão*. Cada tecnologia tem sua cor, sem selos de “planejado”.

[![Debian 13](https://img.shields.io/badge/Debian-13-A81D33?style=flat-square&logo=debian&logoColor=white)](https://www.debian.org/) [![Linux Server](https://img.shields.io/badge/Linux-Server-FCC624?style=flat-square&logo=linux&logoColor=white)](https://kernel.org/) [![Ubuntu 26.04 LTS](https://img.shields.io/badge/Ubuntu-26.04%20LTS-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/) [![Android 5G Client](https://img.shields.io/badge/Android-5G%20Client-3DDC84?style=flat-square&logo=android&logoColor=white)](https://www.android.com/) [![OpenSSH Remote](https://img.shields.io/badge/OpenSSH-Remote-527EA2?style=flat-square)](https://www.openssh.com/)

[![Docker Engine](https://img.shields.io/badge/Docker-Engine-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/) [![Docker Compose](https://img.shields.io/badge/Docker-Compose-1D63ED?style=flat-square&logo=docker&logoColor=white)](https://docs.docker.com/compose/) [![Portainer CE](https://img.shields.io/badge/Portainer-CE-13BEF9?style=flat-square&logo=portainer&logoColor=white)](https://www.portainer.io/) [![Navidrome Audio](https://img.shields.io/badge/Navidrome-Audio-27AFA2?style=flat-square)](https://www.navidrome.org/) [![Heimdall Dashboard](https://img.shields.io/badge/Heimdall-Dashboard-9B64C6?style=flat-square)](https://heimdall.site/) [![Uptime Kuma Monitoring](https://img.shields.io/badge/Uptime%20Kuma-Monitoring-5CDD8B?style=flat-square)](https://github.com/louislam/uptime-kuma) [![Pi-hole DNS](https://img.shields.io/badge/Pi-hole-DNS-96060C?style=flat-square&logo=pihole&logoColor=white)](https://pi-hole.net/) [![Apache HTTP Server](https://img.shields.io/badge/Apache-HTTP%20Server-D22128?style=flat-square&logo=apache&logoColor=white)](https://httpd.apache.org/)

[![Beszel Metrics](https://img.shields.io/badge/Beszel-Metrics-168A72?style=flat-square)](https://beszel.dev/) [![Dozzle Logs](https://img.shields.io/badge/Dozzle-Logs-8266CF?style=flat-square)](https://dozzle.dev/) [![Tailscale Remote Access](https://img.shields.io/badge/Tailscale-Remote%20Access-252525?style=flat-square&logo=tailscale&logoColor=white)](https://tailscale.com/) [![Samba Files](https://img.shields.io/badge/Samba-Files-087E8B?style=flat-square)](https://www.samba.org/) [![Syncthing Sync](https://img.shields.io/badge/Syncthing-Sync-0891D1?style=flat-square&logo=syncthing&logoColor=white)](https://syncthing.net/) [![Gatus Status](https://img.shields.io/badge/Gatus-Status-CD583D?style=flat-square)](https://github.com/TwiN/gatus) [![SFTP Files](https://img.shields.io/badge/SFTP-Files-758C49?style=flat-square)](https://www.openssh.com/) [![Git Versioning](https://img.shields.io/badge/Git-Versioning-F05032?style=flat-square&logo=git&logoColor=white)](https://git-scm.com/) [![Bash Shell](https://img.shields.io/badge/Bash-Shell-4EAA25?style=flat-square&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/) [![rsync Transfer](https://img.shields.io/badge/rsync-Transfer-5C677D?style=flat-square)](https://rsync.samba.org/)

## Como os equipamentos se conectam

~~~text
                           REDE DOMÉSTICA
┌─────────────────────────┐      SSH, rsync, HTTP   ┌──────────────────────────┐
│ Desktop Ubuntu          │◄──────────────────────►│ Notebook Positivo         │
│ i7-3770 / GNOME         │                         │ Debian 13 · 2 GB RAM     │
│ administração + mídias  │                         │ Docker Engine + Compose  │
└─────────────────────────┘                         │ OpenSSH · Tailscale      │
                                                    │ somente eMMC interno    │
                                                    └────────────┬─────────────┘
                                                                 │ rede privada
                                                                 ▼
                                                    ┌──────────────────────────┐
                                                    │ Samsung Galaxy A34 5G    │
                                                    │ acesso e reprodução      │
                                                    └──────────────────────────┘
~~~

O acesso remoto testado não exigiu abrir portas no roteador. Entretanto, o Docker publica a porta **4533** em todas as interfaces do Debian. A revisão de segurança continua pendente; o Tailscale, sozinho, não corrige esse ponto.

## O limite dos recursos

| Medição inicial | Resultado registrado |
|---|---|
| Sistema de arquivos raiz antes do primeiro app | 27G total · 2,0G usados · **23G disponíveis** |
| RAM antes do primeiro contêiner | ~1,8 GiB visíveis · **388 MiB usados** · ~1,4 GiB disponíveis |
| Swap | Partição de 1,5 GiB · 0 B usados |
| Primeira leitura do Navidrome | **32,51 MiB RAM** · 15,83% de CPU na inicialização (amostra pontual) |
| Armazenamento externo | **Nenhum** — uso permanente do Ventoy foi descartado |

O número do contêiner é **uma medição inicial**, não uma média de consumo. Músicas, metadados, imagens e logs disputam espaço com o próprio Debian. Por isso, cada serviço novo precisa justificar o consumo.

## Como reproduzir e estudar

Em um Debian já preparado com Docker Engine e Compose:

~~~bash
git clone https://github.com/kayo-willian/soft-and-wet-homelab.git
cd soft-and-wet-homelab
mkdir -p data/navidrome music
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
sudo docker compose config
sudo docker compose up -d
sudo docker compose ps
~~~

O procedimento reproduz **o primeiro serviço documentado**, não uma stack inteira fictícia. A pasta original do servidor é `~/soft-and-wet`. O Compose continua igual à primeira configuração funcional; os próximos serviços e ajustes de segurança ainda não foram aplicados.

## Documentação técnica

| Área | Português | English |
|---|---|---|
| Navegação | [Índice](docs/pt-BR/README.md) | [Index](docs/README.md) |
| Equipamentos | [Inventário](docs/pt-BR/hardware.md) | [Hardware inventory](docs/hardware.md) |
| Arquitetura | [Arquitetura](docs/pt-BR/architecture.md) | [Architecture](docs/architecture.md) |
| Implantação | [Instalação](docs/pt-BR/installation.md) | [Installation](docs/installation.md) |
| Serviços | [Registro de serviços](docs/pt-BR/services/README.md) | [Service registry](docs/services/README.md) |
| Redes | [Redes e acesso remoto](docs/pt-BR/networking.md) | [Networking](docs/networking.md) |
| Operação | [Operação e backup](docs/pt-BR/operations.md) | [Operations](docs/operations.md) |
| Segurança | [Limites de segurança](docs/pt-BR/security.md) | [Security](docs/security.md) |
| Medições | [Medições reais](docs/pt-BR/measurements.md) | [Measurements](docs/measurements.md) |
| Catálogo | [Coleção Brasil](docs/pt-BR/music-library.md) | [Brasil collection](docs/music-library.md) |
| Problemas | [Solução de problemas](docs/pt-BR/troubleshooting.md) | [Troubleshooting](docs/troubleshooting.md) |

A documentação explica *por quê*, *quais comandos foram usados*, *o que foi realmente observado* e *o que ainda não foi feito*. Não publico senhas, músicas, bancos, IPs/MACs privados nem dados pessoais da máquina.

## E depois?

Este ainda é o começo. Os próximos nomes pertencem a uma sequência maior — mas os detalhes ficam para quando cada fase existir:

<div align="center">

**SOFT & WET**  
↓  
**PLUNDER**  
↓  
**SOFT & WET · V2**  
↓  
**SOFT & WET · V3**  
↓  
**GO BEYOND**  
↓  
**MADE IN HEAVEN**

*Algumas etapas talvez nunca aconteçam. O resto será revelado no momento certo.*

</div>

## Escopo e licença

Este é um projeto técnico pessoal — não um produto de software, serviço comercial ou ambiente empresarial. Sem VM, sem pendrive permanente e sem automação de downloads de filmes. Não afirmo backups testados, segurança de produção nem disponibilidade garantida.

A [licença MIT](LICENSE) cobre a documentação e as configurações originais do repositório, **não** programas de terceiros ou músicas.
