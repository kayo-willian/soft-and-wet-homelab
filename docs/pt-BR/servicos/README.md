# Serviços — instalados e planejados

[English](../../services/README.md) · [Documentação](../README.md) · [Início](../../../README.pt-BR.md)

O Soft & Wet é um **projeto pessoal de infraestrutura**, não um pacote de serviços prontos. Tudo precisa funcionar em um Atom x5-Z8350 com 2 GB de RAM e armazenamento eMMC limitado.

| Componente | Camada | Estado | Objetivo |
| --- | --- | --- | --- |
| [Debian 13](https://www.debian.org/) | Host | Funcionando | Servidor headless administrado por SSH |
| [OpenSSH](https://www.openssh.com/) | Host | Funcionando | Administração e transferência segura |
| [Docker Engine](https://docs.docker.com/engine/) + [Compose](https://docs.docker.com/compose/) | Host | Funcionando | Contêineres e configuração reproduzível |
| [Navidrome](https://www.navidrome.org/) | Contêiner | **Funcionando** | Biblioteca pessoal de músicas |
| [Tailscale](https://tailscale.com/) | Host + Android | **Funcionando** | Acesso remoto privado sem encaminhamento de portas |
| [Portainer CE](https://www.portainer.io/) | Contêiner | **Não instalado** | Administração gráfica que consome RAM e concede acesso privilegiado ao Docker |
| [Uptime Kuma](https://github.com/louislam/uptime-kuma) | Contêiner | **Não instalado** | Monitoramento de disponibilidade e latência |
| [Heimdall](https://heimdall.site/) | Contêiner | **Não instalado** | Atalhos para serviços |
| [Pi-hole](https://pi-hole.net/) | Contêiner | **Não instalado** | Filtragem de DNS que exige cuidado com a rede |
| [Apache HTTP Server](https://httpd.apache.org/) | Contêiner se testado | **Não instalado** | Laboratório HTTP/proxy |
| [Beszel](https://beszel.dev/) | Monitoramento | **Não instalado** | Métricas de CPU/RAM/disco |
| [Dozzle](https://dozzle.dev/) | Logs | **Não instalado** | Consulta de logs do Docker |

## Documentação dos serviços funcionando

- [Navidrome](navidrome.md) — primeiro contêiner, Compose, arquivos e medições.
- [Tailscale](tailscale.md) — instalado diretamente no Debian e no Android, **fora do Docker**.

## Alternativas avaliadas e descartadas

- Plex e Jellyfin: sem necessidade para a biblioteca exclusivamente musical.
- Sonarr, Radarr, Prowlarr, Bazarr e automação de downloads de vídeo: fora do escopo por preferência e armazenamento limitado.
- File Browser: cogitado inicialmente, mas o repositório original foi arquivado; não selecionado.
- Samba, Syncthing, Gatus e zram: discutidos, mas **não implementados**.
- Portainer, Uptime Kuma, Heimdall, Pi-hole, Apache, Beszel e Dozzle continuam como possibilidades, não como parte do ambiente atual.

## Critério para adicionar serviços

Instalar **um por vez**, medir RAM em repouso, CPU em uso, tamanho da imagem e espaço ocupado. Registrar caminhos persistentes e alterações de rede. Manter apenas o que oferecer utilidade proporcional ao custo.

Ver [medições](../medicoes.md), [segurança](../seguranca.md) e [operações](../operacoes.md).
