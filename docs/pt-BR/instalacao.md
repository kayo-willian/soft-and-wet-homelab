# Instalação e preparação

[English](../installation.md) · [Documentação](README.md) · [Início](../../README.pt-BR.md)

> Instalei as ferramentas e o Docker no Positivo. O histórico completo do shell não foi preservado. As etapas de instalação dos pacotes e do Docker reconstituem o procedimento. A seção de verificação identifica os comandos registrados no terminal.

## 1. Debian 13

Instalei o Debian 13 (Trixie) sem interface gráfica. As tarefas recomendadas no instalador foram **Servidor SSH** e **Utilitários de sistema padrão**. Se o SSH ficar de fora da instalação, instale o OpenSSH conforme o exemplo abaixo. Administro o servidor `debian` pela LAN a partir da estação Ubuntu, em outro cômodo.

Os comandos a seguir pressupõem uma conta Debian com acesso a sudo. Confira as atualizações pendentes antes de aceitá-las com `-y`.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  openssh-server git curl wget sudo ca-certificates gnupg htop ncdu \
  nano vim unzip zip tar rsync ufw fail2ban jq cron logrotate \
  smartmontools unattended-upgrades bash-completion iproute2 iputils-ping \
  dnsutils net-tools traceroute tcpdump lsof tree
sudo systemctl enable --now ssh
```

Esta lista é uma **referência para reprodução**, não uma prova de auditoria individual de cada pacote. Não há confirmação de que UFW ou Fail2ban foram configurados ou ativados.

Para uma instalação menor, também considerei `--no-install-recommends` com `openssh-server git curl ca-certificates htop nano`. Optei pela lista mais completa acima.

## 2. Docker Engine e Compose

A instalação utilizou o repositório oficial do Docker para Debian. Em um host novo, confira os pré-requisitos e pacotes conflitantes na referência oficial abaixo. O bloco grava a chave e a definição do repositório nos caminhos indicados.

```bash
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
```

O registro do terminal inclui estas verificações:

```bash
sudo docker system df
sudo docker compose version
```

O Compose reportou **v5.8.0**. Uma instalação posterior pode fornecer outra versão. Na coleta inicial, o Docker tinha zero contêineres e volumes; existia apenas uma imagem muito pequena.

## 3. Pasta de trabalho

Estes são os comandos históricos da primeira implantação, executados em uma pasta nova. Os dois redirecionamentos sobrescrevem `.env` e `.gitignore`. Em um clone novo, siga o [README do projeto](../../README.pt-BR.md) e preserve as regras de exclusão existentes.

```bash
mkdir -p ~/soft-and-wet/{data/navidrome,music,docs,scripts}
cd ~/soft-and-wet
nano compose.yaml
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
printf '.env\ndata/\nmusic/\n' > .gitignore
```

O [.gitignore da raiz](../../.gitignore) amplia as regras usadas na instalação inicial. O repositório se chama `soft-and-wet-homelab` e a pasta de aplicação no servidor é `~/soft-and-wet`.

## 4. Primeiro contêiner

Com o [compose.yaml do repositório](../../compose.yaml) e o `.env` na pasta original do servidor:

```bash
cd ~/soft-and-wet
sudo docker compose up -d
sudo docker compose ps
sudo docker stats --no-stream
```

O terminal mostrou a imagem `navidrome/navidrome:latest` sendo baixada, a rede `soft-and-wet_default` sendo criada e o contêiner `navidrome` iniciando.

## 5. Escopo da primeira instalação

O servidor continuou sem interface gráfica, Apache ou pendrive permanente. A partição swap de 1,5 GiB foi mantida. O Navidrome foi o único contêiner de aplicação. Portainer, Uptime Kuma e Heimdall permaneceram como possibilidades.

## Próximos passos

- [Implantar o Navidrome](servicos/navidrome.md)
- [Transferir a biblioteca musical](biblioteca-musical.md)
- [Conectar o Android pelo Tailscale](servicos/tailscale.md)

## Referências oficiais

- [Instalação do Docker Engine no Debian](https://docs.docker.com/engine/install/debian/)
- [Instalação do Navidrome com Docker](https://www.navidrome.org/docs/installation/docker/)
