# Soft & Wet — Homelab pessoal com Debian

[English README](README.md)

Um laboratório doméstico sem interface gráfica, montado em um notebook Positivo de recursos limitados. Uso esse ambiente para praticar administração Linux, Docker Compose, redes, acesso remoto e serviços self-hosted, documentando apenas o que foi realmente executado.

## Ambiente

| Componente | Configuração |
| --- | --- |
| Máquina | Positivo com Intel Atom x5-Z8350 |
| RAM | 2 GB físicos (aproximadamente 1,8 GiB informados pelo Linux) |
| Armazenamento | Aproximadamente 29 GB internos; sistema de arquivos raiz de 27 GB |
| Sistema | Debian 13, sem interface gráfica |
| Hostname | `debian` |
| Administração | OpenSSH |
| Contêineres | Docker Engine e Docker Compose |
| Mídias | Armazenamento interno, sem pendrive permanente |

## O que está funcionando

- **Navidrome:** servidor de música em um contêiner Docker; coleção **Brasil** com 10 músicas.
- **Tailscale:** instalado no Debian e no Android; acesso ao Navidrome fora da rede local confirmado.
- **SSH:** administração remota a partir do Ubuntu.

**Ainda não instalados:** Portainer, Uptime Kuma, Heimdall, Pi-hole e Apache. Não apresento esses componentes como concluídos.

## Documentação

- [Arquitetura](docs/architecture.md)
- [Instalação e preparação](docs/installation.md)
- [Configuração do Navidrome](docs/services/navidrome.md)
- [Acesso remoto com Tailscale](docs/services/tailscale.md)
- [Rede e portas](docs/networking.md)
- [Biblioteca Brasil](docs/music-library.md)
- [Medições](docs/measurements.md)
- [Operação](docs/operations.md)
- [Segurança](docs/security.md)
- [Solução de problemas](docs/troubleshooting.md)
- [Histórico](CHANGELOG.md)

Os documentos técnicos detalhados estão em inglês para manter uma única fonte de informação e evitar versões divergentes. Músicas, senhas, banco de dados, cache e informações privadas não são publicados.

## Reproduzir o Navidrome

Após preparar Docker e Compose no Debian, copie `compose.yaml` e `.env.example` do repositório para `~/soft-and-wet`, crie as pastas de dados e música, gere o `.env` com seu UID/GID e execute `sudo docker compose up -d`. O procedimento completo está em [installation.md](docs/installation.md).

O projeto continua em desenvolvimento. Os próximos serviços serão escolhidos conforme o consumo real de RAM e armazenamento.
