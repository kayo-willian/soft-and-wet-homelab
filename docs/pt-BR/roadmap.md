# Roadmap — Soft & Wet → Go Beyond → Made in Heaven

[English](../roadmap.md) · [Documentação](README.md)

Este é o roadmap definido para o homelab pessoal. Cada fase representa um **objetivo técnico**, não apenas a substituição de um computador. Os nomes e limites seguem o roadmap original do projeto.

| Fase | Repositório / identidade | Ambiente | Objetivo | Compromisso |
|---|---|---|---|---|
| **01 — Fase inicial** | `soft-and-wet-homelab` | Positivo, Debian 13, 2 GB RAM | Primeiro servidor headless; SSH, Docker/Compose, Navidrome, serviços leves e registro de consumo/falhas | **Atual** |
| **02 — Experimento paralelo** | `soft-and-wet-plunder` | Mesmo Positivo, Alpine Linux | Explorar um sistema mais enxuto, OpenRC, contêineres e o mínimo necessário; preservar os dados antes de reinstalar | Experimento opcional, não simultâneo ao Debian |
| **03 — Evolução condicional** | `soft-and-wet-homelab-v2` | Notebook melhor, SO indefinido | Experimentar Ubuntu Server ou UmbrelOS (se compatível) e ampliar serviços | Depende de hardware |
| **04 — Etapa opcional** | `soft-and-wet-homelab-v3` | Hardware/SO indefinidos | Nova arquitetura ou desafio relevante | Pode ser pulada |
| **05 — Grande salto** | `go-beyond` | Desktop i7-3770 reaproveitado | Quando houver outro PC principal, transformar o desktop atual em servidor dedicado, melhorando armazenamento, serviços, redes, monitoramento e backups | Futuro |
| **06 — Visão de longo prazo** | `made-in-heaven` | Vários equipamentos, sem hardware definido | NAS, virtualização, segmentação e redundância | Sem prazo |

## Fase atual: Soft & Wet

O primeiro marco é um **Navidrome com Tailscale funcionando** no navegador local e no Android fora de casa. Isso não conclui todo o Homelab: backups, endurecimento de acesso, novos serviços e medições prolongadas continuam pendentes.

## Limites do planejamento

- `soft-and-wet-plunder` é um **experimento separado com Alpine/OpenRC**, não outro sistema já instalado em paralelo. Preservar dados antes de substituir o Debian.
- V2 e V3 seriam ambientes e repositórios distintos, **não versões já implantadas**.
- O desktop i7-3770 continua como PC principal até uma substituição futura; só depois entraria no `go-beyond`.
- Sem datas obrigatórias ou necessidade de criar os seis repositórios.
- Este roadmap indica o **caminho de aprendizado**, não um calendário de lançamento.

A identidade geral é **Soft & Wet → Go Beyond → Made in Heaven**, com etapas intermediárias flexíveis.
