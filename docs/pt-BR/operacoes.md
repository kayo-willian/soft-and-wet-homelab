# Operação e manutenção

[English](../operations.md) · [Documentação](README.md) · [Início](../../README.pt-BR.md)

## Acesso administrativo

No Ubuntu, substitua os valores de exemplo pela conta e pelo endereço do Debian na rede local.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
ssh "${DEBIAN_USER}@${DEBIAN_HOST}"
```

O hostname atual é `debian`; o nome curto só resolve automaticamente se houver DNS/mDNS adequado na rede. O IP local é a alternativa direta.

## Ciclo de vida do Navidrome

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker compose restart navidrome
sudo docker stats --no-stream
```

O comando `docker compose down` remove contêiner e rede Compose, porém os dados em bind mounts continuam no host. Evitar apagar arquivos persistentes sem backup e verificação.

## Atualização controlada (ainda não executada)

O `compose.yaml` original usa `navidrome/navidrome:latest`. Antes de atualizar, analisar a versão e fazer backup independente dos dados. Exemplo de atualização futura:

```bash
cd ~/soft-and-wet
sudo docker compose pull navidrome
sudo docker compose up -d navidrome
sudo docker compose ps
```

Não há registro de atualização da imagem após a instalação inicial. Fixar uma versão específica pode ser uma melhoria posterior documentada à parte.

## Transferir músicas novas

No Ubuntu, substitua a conta e o IP de exemplo abaixo pelo login e pelo endereço do Debian na LAN. O caminho remoto parte da pasta pessoal dessa conta. As duas máquinas precisam de `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
```

No Debian, acompanhar:

```bash
du -sh ~/soft-and-wet/music
free -h
df -h /
```

## Backup — plano, não implementação

O eMMC interno é ponto único de falha. O procedimento futuro recomendado é interromper as gravações do Navidrome, copiar `data/navidrome/` e `music/` para **outro computador confiável**, e testar a restauração. Copiar dentro do mesmo eMMC não constitui backup independente.

Não há script de backup, rotina agendada, destino externo nem restauração comprovada.

## Diagnóstico geral

```bash
cd ~/soft-and-wet
sudo systemctl status ssh docker tailscaled
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
tailscale status
sudo docker system df
sudo journalctl -u docker -n 50 --no-pager
```

Cada novo serviço deverá ter registro de consumo, portas, persistência, logs e procedimento de recuperação.
