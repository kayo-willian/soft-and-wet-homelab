# Inventário de hardware — três papéis, um servidor pequeno

[English](../hardware.md) · [Índice](README.md)

Este inventário registra os **equipamentos realmente envolvidos** no Soft & Wet. O Positivo é o servidor. O Ubuntu é a estação de administração e transferência de arquivos. O Android é o cliente remoto. Não formam um cluster, e os outros dois dispositivos não são hosts Docker deste projeto.

## 1. Servidor físico — Positivo

| Componente | Valor registrado |
|---|---|
| Papel | **Servidor Soft & Wet** |
| Equipamento | Notebook Positivo |
| Processador | **Intel Atom x5-Z8350** |
| Memória física | **2 GB RAM** (~1,8 GiB visíveis ao Linux) |
| Armazenamento interno | ~29 GB nominais; partição raiz de **27G** |
| Partição raiz | `/dev/mmcblk2p2` |
| Partição swap | `/dev/mmcblk2p3`, **1,5 GiB** |
| Sistema | **Debian 13 (Trixie)** sem ambiente gráfico |
| Hostname | `debian` |
| Administração | OpenSSH; operação headless em outro cômodo |
| Contêineres | Docker Engine e **Docker Compose v5.8.0** na primeira medição |
| Mídias | Apenas eMMC interno; **sem pendrive Ventoy conectado permanentemente** |

Na primeira coleta, `df -h /` mostrou **27G no total, 2,0G utilizados e 23G disponíveis**. O `free -h` registrou **388 MiB usados e ~1,4 GiB disponíveis**; swap em 0. Esses valores são **anteriores ao primeiro contêiner**, não refletem necessariamente o consumo atual.

O projeto não presume discos redundantes, nobreak, backup externo ou aceleração gráfica dedicada. Foi discutido manter a tampa fechada sem suspender, mas a configuração persistente de energia não foi comprovada.

## 2. Estação de administração — desktop Ubuntu

Dados fornecidos em **23/11/2026** pelas saídas de `./info.sh`, `ip a`, `df -h /` e `free -h`.

| Componente | Especificação registrada |
|---|---|
| Papel | SSH, navegador, organização de arquivos e `rsync` |
| Processador | **Intel Core i7-3770**, 4 núcleos físicos / **8 threads** |
| Vídeo | **Intel HD Graphics 4000** integrada |
| Memória | ~**16 GB instalados**, **14,83 GiB reportados** |
| Sistema | **Ubuntu 26.04.1 LTS** |
| Kernel | **Linux 7.0.0-44-generic** |
| Ambiente gráfico | **GNOME 50.1**, Mutter no **Wayland** |
| Shell | **Bash 5.3.9** |
| Sistema de arquivos raiz | `/dev/sda2`, ~**109,47 GiB** totais no relatório |
| Ocupação observada | ~**33,53 GiB usados**, ~**71 GiB disponíveis** no `df` |
| Swap | **4,00 GiB**, 0 B em uso |
| Conexão | Ethernet e adaptador Wi-Fi USB conectados à rede doméstica; endereços omitidos |

A pequena diferença entre arredondamentos de memória e armazenamento decorre das unidades e dos comandos utilizados. É um **registro pontual**, não uma garantia do kernel, uso de disco ou RAM atuais.

### Função do Ubuntu

- Administrar o Debian por SSH sem monitor conectado ao servidor.
- Transferir músicas de `~/Músicas/Brasil/` para o Positivo utilizando `rsync`.
- Testar as interfaces web pela rede local.
- O hardware mais forte do desktop **não aumenta** a RAM ou o armazenamento do Positivo; o Soft & Wet continua limitado aos próprios recursos.

## 3. Cliente móvel — Samsung Galaxy A34 5G

| Componente | Especificação |
|---|---|
| Papel | Cliente remoto de serviços |
| Modelo | **Samsung Galaxy A34 5G** |
| Versão do aparelho | **6 GB RAM / 128 GB armazenamento** |
| Processador | **MediaTek Dimensity 1080** (especificação do modelo) |
| Tela | **Super AMOLED de 6,6″, 120 Hz** |
| Bateria | **5.000 mAh** |
| Rede | Wi-Fi e dados móveis **5G** |
| Acesso testado | Tailscale e aplicativo compatível com Navidrome |

Os dados do modelo foram conferidos no [Samsung Mobile Press](https://samsungmobilepress.com/media-assets/galaxy-a34-5g?tab=specs) e no [Samsung Newsroom Brasil](https://news.samsung.com/br/samsung-galaxy-a34-5g-esta-disponivel-em-todo-o-brasil). A variante 6/128 GB e o streaming remoto são informações confirmadas pelo usuário. A versão exata do Android/One UI **não foi informada**, portanto não é presumida.

## Redes e privacidade

IPs locais, pontes Docker, IPs do Tailscale e endereços MAC do terminal **não foram publicados** neste inventário. O desenho de rede está em [redes](networking.md); cada pessoa deve utilizar os próprios endereços.

Veja [arquitetura](architecture.md) para entender os papéis dos equipamentos e [medições](measurements.md) para os resultados coletados.
