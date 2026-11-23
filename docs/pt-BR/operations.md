# Operação e manutenção

[English](../operations.md) · [Índice](README.md)

## Acesso administrativo

A administração acontece a partir do Ubuntu:

~~~bash
ssh kayo@<DEBIAN_LAN_IP>
~~~

O hostname atual é `debian`; o nome curto só resolve automaticamente se houver DNS/mDNS adequado na rede. O IP local é a alternativa direta.

## Ciclo de vida do Navidrome

~~~bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker compose restart navidrome
sudo docker stats --no-stream
~~~

O comando `docker compose down` remove contêiner e rede Compose, porém os dados em bind mounts continuam no host. Evitar apagar arquivos persistentes sem backup e verificação.

## Atualização controlada (ainda não executada)

O `compose.yaml` original usa `navidrome/navidrome:latest`. Antes de atualizar, analisar a versão e fazer backup independente dos dados. Exemplo de atualização futura:

~~~bash
cd ~/soft-and-wet
sudo docker compose pull navidrome
sudo docker compose up -d navidrome
sudo docker compose ps
~~~

Não há registro de atualização da imagem após a instalação inicial. Fixar uma versão específica pode ser uma melhoria posterior documentada à parte.

## Transferir músicas novas

No Ubuntu:

~~~bash
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  kayo@<DEBIAN_LAN_IP>:/home/kayo/soft-and-wet/music/Brasil/
~~~

No Debian, acompanhar:

~~~bash
du -sh ~/soft-and-wet/music
free -h
df -h /
~~~

## Backup — plano, não implementação

O eMMC interno é ponto único de falha. O procedimento futuro recomendado é interromper as gravações do Navidrome, copiar `data/navidrome/` e `music/` para **outro computador confiável**, e testar a restauração. Copiar dentro do mesmo eMMC não constitui backup independente.

Não há script de backup, rotina agendada, destino externo nem restauração comprovada. Não usar `docker compose down -v` sem entender os efeitos sobre dados e volumes.

## Diagnóstico geral

~~~bash
sudo systemctl status ssh docker tailscaled
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
tailscale status
sudo docker system df
journalctl -u docker -n 50 --no-pager
~~~

Cada novo serviço deverá ter registro de consumo, portas, persistência, logs e procedimento de recuperação.
