# Instalação e preparação

[English](../installation.md) · [Índice](README.md)

> **Limite das evidências:** parte dos comandos abaixo foi oferecida durante a instalação inicial. O usuário confirmou que instalou as ferramentas e o Docker, mas o histórico completo do shell não foi preservado. As saídas realmente apresentadas são identificadas como verificações.

## 1. Debian 13

Instalei **Debian 13 (Trixie)** no notebook Positivo, sem interface gráfica. Na seleção de tarefas, foram recomendados apenas **Servidor SSH** e **Utilitários de sistema padrão**, sem Apache nem ambiente desktop. O hostname é `debian` e o equipamento fica em outro cômodo, administrado pelo Ubuntu.

A lista inicial de ferramentas recomendada foi:

~~~bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  openssh-server git curl wget sudo ca-certificates gnupg htop ncdu \
  nano vim unzip zip tar rsync ufw fail2ban jq cron logrotate \
  smartmontools unattended-upgrades bash-completion iproute2 iputils-ping \
  dnsutils net-tools traceroute tcpdump lsof tree
sudo systemctl enable --now ssh
~~~

Esta lista é uma **referência para reprodução**, não uma prova de auditoria individual de cada pacote. Não há confirmação de que UFW ou Fail2ban foram configurados ou ativados.

Uma versão mínima também foi discutida (`openssh-server git curl ca-certificates htop nano` com `--no-install-recommends`), mas o usuário declarou ter instalado o conjunto mais completo.

## 2. Docker Engine e Compose

O procedimento fornecido utilizou o repositório oficial para Debian:

~~~bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg \
  -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
~~~

Verificações que apareceram efetivamente no terminal:

~~~bash
sudo docker system df
sudo docker compose version
~~~

O Compose reportou **v5.8.0**. Na coleta inicial, o Docker tinha zero contêineres e volumes; existia apenas uma imagem muito pequena.

## 3. Pasta de trabalho

Os comandos executados para o primeiro serviço foram:

~~~bash
mkdir -p ~/soft-and-wet/{data/navidrome,music,docs,scripts}
cd ~/soft-and-wet
nano compose.yaml
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
printf '.env\ndata/\nmusic/\n' > .gitignore
~~~

O `.gitignore` inicial era pequeno; o repositório ganhou uma versão ampliada e segura para publicação. A pasta de aplicação no host continua `~/soft-and-wet`.

## 4. Primeiro contêiner

Após criar o `compose.yaml` do [Navidrome](services/navidrome.md):

~~~bash
cd ~/soft-and-wet
sudo docker compose up -d
sudo docker compose ps
sudo docker stats --no-stream
~~~

O terminal mostrou a imagem `navidrome/navidrome:latest` sendo baixada, a rede `soft-and-wet_default` sendo criada e o contêiner `navidrome` iniciando.

## 5. O que não foi feito nesta etapa

- Não instalamos Apache no host.
- Não iniciamos Portainer, Uptime Kuma ou Heimdall.
- Não alteramos a partição swap de 1,5 GB.
- Não conectamos pendrive permanentemente.
- Não há registro de instalação de ferramentas gráficas no servidor.

Referências: [Docker Debian](https://docs.docker.com/engine/install/debian/) e [Navidrome Docker](https://www.navidrome.org/docs/installation/docker/).
