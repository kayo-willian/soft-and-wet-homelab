# Histórico — Soft & Wet

[English](CHANGELOG.md) · [Documentação](docs/pt-BR/README.md) · [Início](README.pt-BR.md)

Registro das etapas **efetivamente realizadas**, sem apresentar serviços planejados como instalados.

## 23/11/2026 — Primeiro serviço funcional

- Confirmei o Debian 13 sem interface gráfica em outro cômodo, administrado remotamente por SSH.
- Verifiquei Docker e Compose; a versão informada foi `v5.8.0`.
- Medi memória, swap e armazenamento antes de iniciar serviços de aplicação.
- Criei `~/soft-and-wet`, `compose.yaml`, `.env` com UID/GID e `.gitignore`.
- Baixei `navidrome/navidrome:latest` e iniciei um contêiner na rede `soft-and-wet_default`.
- Acessei a interface inicial do Navidrome na porta TCP 4533 e configurei a conta administrativa.
- Transferi dez músicas da coleção **Brasil**, organizadas em uma pasta própria, e as vi na biblioteca do Navidrome.
- Encontrei `Unknown Album` em algumas faixas e experimentei o MusicBrainz Picard. Preferi utilizar a seção **Músicas** sem alterar metadados não comprovados.
- Instalei e autentiquei o Tailscale no Debian e no Samsung A34. Depois, confirmei a reprodução remota no celular.

## Pendências

- Portainer, Heimdall, Uptime Kuma, Pi-hole, Apache e demais contêineres.
- Backup independente e teste de restauração.
- Autenticação SSH exclusivamente por chave e revisão das interfaces/portas publicadas.
- Medições prolongadas de consumo após a biblioteca de músicas.
- Scripts próprios de automação, ainda não implementados.

Para contexto técnico, consulte [instalação](docs/pt-BR/instalacao.md), [solução de problemas](docs/pt-BR/solucao-de-problemas.md) e [planejamento](docs/pt-BR/planejamento.md).

## Alterações na documentação

- Os arquivos em português receberam nomes localizados. Cada página passou a ter links para a tradução, o índice e o README do projeto.
