<div align="center">

![Soft & Wet — capa do homelab em estilo mangá](assets/soft-and-wet-cover.webp)

# Soft & Wet

[English](README.md) · [Documentação](docs/pt-BR/README.md) · [Início](README.pt-BR.md)

**Um servidor doméstico real construído com um notebook praticamente obsoleto.**

[![Licença MIT](https://img.shields.io/static/v1?label=Licen%C3%A7a&message=MIT&color=A0A0A0&style=flat-square&labelColor=161B22)](LICENSE)

**Em uso no laboratório**

[![Debian 13](https://img.shields.io/static/v1?label=Debian&message=13&color=A81D33&style=flat-square&labelColor=161B22&logo=debian&logoColor=white)](https://www.debian.org/) [![Linux Servidor](https://img.shields.io/static/v1?label=Linux&message=Servidor&color=D6B400&style=flat-square&labelColor=161B22&logo=linux&logoColor=white)](https://kernel.org/) [![Ubuntu 26.04 LTS](https://img.shields.io/static/v1?label=Ubuntu&message=26.04%20LTS&color=E95420&style=flat-square&labelColor=161B22&logo=ubuntu&logoColor=white)](https://ubuntu.com/) [![Android Cliente](https://img.shields.io/static/v1?label=Android&message=Cliente&color=3DDC84&style=flat-square&labelColor=161B22&logo=android&logoColor=white)](https://www.android.com/) [![OpenSSH Acesso remoto](https://img.shields.io/static/v1?label=OpenSSH&message=Acesso%20remoto&color=527EA2&style=flat-square&labelColor=161B22)](https://www.openssh.com/) [![Docker Engine](https://img.shields.io/static/v1?label=Docker&message=Engine&color=2496ED&style=flat-square&labelColor=161B22&logo=docker&logoColor=white)](https://www.docker.com/) [![Docker Compose](https://img.shields.io/static/v1?label=Docker&message=Compose&color=1D63ED&style=flat-square&labelColor=161B22&logo=docker&logoColor=white)](https://docs.docker.com/compose/) [![Navidrome Áudio](https://img.shields.io/static/v1?label=Navidrome&message=%C3%81udio&color=27AFA2&style=flat-square&labelColor=161B22)](https://www.navidrome.org/) [![Tailscale Acesso](https://img.shields.io/static/v1?label=Tailscale&message=Acesso&color=333333&style=flat-square&labelColor=161B22)](https://tailscale.com/) [![Git Versionamento](https://img.shields.io/static/v1?label=Git&message=Versionamento&color=F05032&style=flat-square&labelColor=161B22&logo=git&logoColor=white)](https://git-scm.com/) [![Bash Shell](https://img.shields.io/static/v1?label=Bash&message=Shell&color=4EAA25&style=flat-square&labelColor=161B22&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/) [![rsync Transferência](https://img.shields.io/static/v1?label=rsync&message=Transfer%C3%AAncia&color=5C677D&style=flat-square&labelColor=161B22)](https://rsync.samba.org/)

**Ferramentas avaliadas ou com configuração pendente**

[![Portainer CE](https://img.shields.io/static/v1?label=Portainer&message=CE&color=13BEF9&style=flat-square&labelColor=161B22)](https://www.portainer.io/) [![Heimdall Painel](https://img.shields.io/static/v1?label=Heimdall&message=Painel&color=9B64C6&style=flat-square&labelColor=161B22)](https://heimdall.site/) [![Uptime Kuma Monitoramento](https://img.shields.io/static/v1?label=Uptime%20Kuma&message=Monitoramento&color=4ABF76&style=flat-square&labelColor=161B22)](https://github.com/louislam/uptime-kuma) [![Pi-hole DNS](https://img.shields.io/static/v1?label=Pi-hole&message=DNS&color=96060C&style=flat-square&labelColor=161B22)](https://pi-hole.net/) [![Apache — servidor HTTP](https://img.shields.io/static/v1?label=Apache&message=Servidor%20HTTP&color=D22128&style=flat-square&labelColor=161B22&logo=apache&logoColor=white)](https://httpd.apache.org/) [![Beszel Métricas](https://img.shields.io/static/v1?label=Beszel&message=M%C3%A9tricas&color=168A72&style=flat-square&labelColor=161B22)](https://beszel.dev/) [![Dozzle Logs](https://img.shields.io/static/v1?label=Dozzle&message=Logs&color=8266CF&style=flat-square&labelColor=161B22)](https://dozzle.dev/) [![Samba Arquivos](https://img.shields.io/static/v1?label=Samba&message=Arquivos&color=087E8B&style=flat-square&labelColor=161B22)](https://www.samba.org/) [![Syncthing Sincronização](https://img.shields.io/static/v1?label=Syncthing&message=Sincroniza%C3%A7%C3%A3o&color=0891D1&style=flat-square&labelColor=161B22&logo=syncthing&logoColor=white)](https://syncthing.net/) [![Gatus Disponibilidade](https://img.shields.io/static/v1?label=Gatus&message=Disponibilidade&color=CD583D&style=flat-square&labelColor=161B22)](https://github.com/TwiN/gatus) [![SFTP Transferência](https://img.shields.io/static/v1?label=SFTP&message=Transfer%C3%AAncia&color=758C49&style=flat-square&labelColor=161B22)](https://www.openssh.com/) [![UFW Firewall](https://img.shields.io/static/v1?label=UFW&message=Firewall&color=B56E28&style=flat-square&labelColor=161B22)](https://wiki.ubuntu.com/UncomplicatedFirewall) [![Fail2ban Proteção](https://img.shields.io/static/v1?label=Fail2ban&message=Prote%C3%A7%C3%A3o&color=B02E5E&style=flat-square&labelColor=161B22)](https://github.com/fail2ban/fail2ban)

O [registro de serviços](docs/pt-BR/servicos/README.md) detalha o estado de cada serviço. Usei `rsync` para transferir músicas. O backup e a ativação de UFW e Fail2ban continuam sem confirmação.

</div>

---

## O experimento

Eu tinha um notebook Positivo com **2 GB de RAM**, processador Intel Atom e aproximadamente **29 GB de eMMC interno**. Sem servidor dedicado, sem NAS, sem recursos para virtualização pesada e sem espaço para deixar mais um computador ocupando minha mesa.

Em vez de descartar o notebook, instalei **Debian 13 sem ambiente gráfico**, coloquei a máquina em outro cômodo e comecei a executar serviços úteis, administrados remotamente por SSH a partir do Ubuntu e acessados também pelo celular.

**O foco do projeto é fazer um hardware extremamente limitado funcionar como servidor de verdade.** Contêineres, música e acesso remoto são experiências para testar essa ideia, não o objetivo final do repositório. Quero conhecer o sistema, medir o custo de cada serviço, resolver problemas, documentar decisões e expandir somente quando os recursos permitirem.

O nome e a identidade visual são inspirados em *JoJolion*. O laboratório começa pequeno, mas sem limitar suas possibilidades futuras.

## Os três dispositivos

| Papel | Equipamento | Especificações principais |
| --- | --- | --- |
| **Servidor — Soft & Wet** | Notebook Positivo | Intel Atom x5-Z8350 · **2 GB de RAM** · ~29 GB de eMMC · **Debian 13**, sem interface gráfica |
| **Administração / origem dos arquivos** | Desktop Ubuntu | **Intel Core i7-3770 (4 núcleos / 8 threads)** · **Intel HD Graphics 4000** · ~16 GB de RAM · Ubuntu **26.04.1 LTS** · GNOME 50.1 / Wayland |
| **Cliente remoto** | Samsung Galaxy A34 5G | **6 GB de RAM / 128 GB de armazenamento** · tela Super AMOLED 6,6″ 120 Hz · 5G · bateria de 5.000 mAh |

O desktop **não é o servidor**: ele serve para acessar por SSH, transferir arquivos e administrar os serviços pelo navegador. O Android é o cliente remoto. O [inventário completo](docs/pt-BR/inventario.md) registra versões, kernel, disco e fontes das especificações.

## O que eu já fiz

| Marco | Implementação | Evidência |
| --- | --- | --- |
| Preparei o sistema mínimo | Debian 13 sem desktop | Servidor operando por SSH em outro cômodo |
| Configurei administração remota | OpenSSH e ferramentas Linux | Acesso pelo Ubuntu |
| Adotei contêineres | Docker Engine + Compose | Compose informou a versão v5.8.0 |
| Testei uma aplicação real | Navidrome e dez músicas locais | Biblioteca visível e reprodução confirmada no navegador |
| Testei acesso fora de casa | Tailscale no Debian e Android | Streaming remoto confirmado |
| Medi os recursos | `df`, `free`, `swapon`, `docker stats` | 23 GiB livres antes dos contêineres; 388 MiB de RAM usados |

O Navidrome é o único contêiner de aplicação documentado. O Tailscale roda diretamente no Debian. A expansão está pausada. O [registro de serviços](docs/pt-BR/servicos/README.md) lista as ferramentas avaliadas para uso futuro.

## Como os equipamentos se conectam

```text
Desktop Ubuntu (SSH, rsync, navegador)
       │ rede doméstica
       ▼
Positivo · Debian 13 · eMMC interno
       ├── OpenSSH
       ├── Docker Engine + Compose → Navidrome (TCP 4533)
       └── Tailscale
              │ rede privada do Tailscale
              ▼
       Galaxy A34 5G (reprodução remota)
```

O teste de reprodução remota funcionou sem encaminhar portas no roteador. O Docker publica a porta TCP **4533** em todas as interfaces do servidor. Restringir esse acesso na LAN continua pendente.

## O limite dos recursos

| Medição inicial | Resultado registrado |
| --- | --- |
| Sistema de arquivos raiz antes do primeiro app | 27 GiB no total · 2,0 GiB usados · **23 GiB disponíveis** |
| RAM antes do primeiro contêiner | ~1,8 GiB visíveis · **388 MiB usados** · ~1,4 GiB disponíveis |
| Swap | Partição de 1,5 GiB · 0 B usados |
| Primeira leitura do Navidrome | **32,51 MiB de RAM** · 15,83% de CPU na inicialização (amostra pontual) |
| Armazenamento externo | **Nenhum** — uso permanente do Ventoy foi descartado |

A [primeira leitura do Navidrome](docs/pt-BR/medicoes.md#primeira-inicialização-do-navidrome) foi feita durante a inicialização. Músicas, metadados, imagens de contêiner e logs disputam espaço com o Debian. Por isso, meço o consumo antes de acrescentar serviços.

## Como reproduzir e estudar

No Debian, use uma conta comum com Git, sudo, Docker Engine e o plugin Compose. Execute uma vez para uma instalação nova em que `~/soft-and-wet` ainda não exista. O comando cria um `.env` local com os IDs da conta proprietária das pastas montadas.

```bash
git clone https://github.com/kayo-willian/soft-and-wet-homelab.git ~/soft-and-wet &&
cd ~/soft-and-wet &&
mkdir -p data/navidrome music &&
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env &&
sudo docker compose config &&
sudo docker compose up -d &&
sudo docker compose ps
```

Esse procedimento inicia o Navidrome com a [configuração Compose original](compose.yaml). O banco e as músicas ficam no host. O [guia de instalação](docs/pt-BR/instalacao.md) descreve a primeira implantação.

## Documentação técnica

| Área | Documento |
| --- | --- |
| Navegação | [Índice](docs/pt-BR/README.md) |
| Equipamentos | [Inventário](docs/pt-BR/inventario.md) |
| Arquitetura | [Arquitetura](docs/pt-BR/arquitetura.md) |
| Implantação | [Instalação](docs/pt-BR/instalacao.md) |
| Serviços | [Registro de serviços](docs/pt-BR/servicos/README.md) |
| Serviços | [Navidrome](docs/pt-BR/servicos/navidrome.md) |
| Serviços | [Tailscale](docs/pt-BR/servicos/tailscale.md) |
| Redes | [Redes e acesso remoto](docs/pt-BR/redes.md) |
| Operação | [Operação e backup](docs/pt-BR/operacoes.md) |
| Segurança | [Limites de segurança](docs/pt-BR/seguranca.md) |
| Medições | [Medições reais](docs/pt-BR/medicoes.md) |
| Catálogo | [Coleção Brasil](docs/pt-BR/biblioteca-musical.md) |
| Problemas | [Solução de problemas](docs/pt-BR/solucao-de-problemas.md) |
| Evolução | [Planejamento](docs/pt-BR/planejamento.md) |
| Marcos | [Histórico](HISTORICO.pt-BR.md) |
| Automação | [Scripts](scripts/README.pt-BR.md) |

A [documentação de segurança](docs/pt-BR/seguranca.md) explica quais dados locais manter fora do Git.

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
