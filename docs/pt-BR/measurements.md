# Medições de recursos

[English](../measurements.md) · [Índice](README.md)

Os números foram coletados via SSH no Debian em **23/11/2026**. Podem variar conforme a carga; valores de inicialização não equivalem a médias de operação.

## Antes de instalar o Navidrome

| Métrica | Valor observado |
|---|---|
| Sistema de arquivos raiz | `/dev/mmcblk2p2`: 27G no total, 2,0G usados, 23G disponíveis, 8% de uso |
| RAM | 1,8 GiB no total; 388 MiB usados; 884 MiB livres; 1,4 GiB disponíveis |
| Swap | 1,5 GiB total; 0 B em uso |
| Partição swap | `/dev/mmcblk2p3` |
| Imagens Docker | 1 imagem, 36,94 kB reportados |
| Contêineres | 0 |
| Volumes locais | 0 |
| Cache de build | 0 |
| Docker Compose | `v5.8.0` |

Comandos utilizados:

~~~bash
df -h /
free -h
sudo docker system df
sudo docker compose version
sudo /usr/sbin/swapon --show
~~~

## Primeira inicialização do Navidrome

A leitura de `sudo docker stats --no-stream`, realizada poucos segundos após iniciar o contêiner, mostrou:

| Métrica | Valor |
|---|---|
| Contêiner | `navidrome` |
| Memória | **32,51 MiB** (1,77% do limite reportado) |
| CPU | **15,83%** no instante da coleta |
| Processos/PIDs | 13 |
| Estado | Running |
| Porta | TCP 4533 |

**Não são médias de longo prazo.** Indexação e reprodução de músicas podem alterar o consumo.

## Medições ainda necessárias

- Uso de disco depois da transferência das dez músicas.
- Memória estável do Navidrome e consumo do processo Tailscale.
- Disponibilidade, tempo ligado, temperatura e energia durante vários dias.
- Impacto de futuros contêineres (nenhum instalado até aqui).

Repetir após cada serviço:

~~~bash
free -h
df -h /
sudo docker stats --no-stream
sudo docker system df
du -sh ~/soft-and-wet/music ~/soft-and-wet/data/navidrome
~~~
