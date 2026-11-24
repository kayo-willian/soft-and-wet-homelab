# Installation and preparation

[Português (Brasil)](pt-BR/instalacao.md) · [Documentation](README.md) · [Home](../README.md)

> I installed the tools and Docker on the Positivo. The full shell history was not retained. The package and Docker setup steps below reconstruct the procedure. The verification section identifies commands captured in the terminal.

## 1. Debian 13 base system

I installed Debian 13 (Trixie) without a desktop. The recommended installer tasks were **SSH server** and **standard system utilities**. If SSH was omitted, install OpenSSH on the host as shown below. The host is named `debian` and is administered over the LAN from the Ubuntu workstation in another room.

The following commands assume the Debian account already has sudo access. Review pending upgrades before accepting them with `-y`.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  openssh-server git curl wget sudo ca-certificates gnupg htop ncdu \
  nano vim unzip zip tar rsync ufw fail2ban jq cron logrotate \
  smartmontools unattended-upgrades bash-completion iproute2 iputils-ping \
  dnsutils net-tools traceroute tcpdump lsof tree
sudo systemctl enable --now ssh
```

This is a **reproducible package list**, not proof that every optional package or service was audited individually. No claim is made that UFW or Fail2ban has been configured or enabled.

For a smaller installation, I also considered `--no-install-recommends` with `openssh-server git curl ca-certificates htop nano`. I used the fuller package list above.

## 2. Docker Engine and Compose plugin

The setup used Docker’s official Debian repository. On a new host, check the official instructions below for prerequisites and conflicting packages. The following block writes the key and repository definition to the indicated paths.

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

The terminal record includes these checks:

```bash
sudo docker system df
sudo docker compose version
```

The actual reported Compose version was `v5.8.0`. Docker was nearly empty at the first measurement: no running containers, no local volumes, and one tiny image. Installing the Compose plugin in the future may yield a newer version.

## 3. Project directory

These are the historical commands from the first deployment, run in a new directory. The two redirections overwrite `.env` and `.gitignore`. For a fresh clone, use the [project README](../README.md) and keep its existing ignore rules.

```bash
mkdir -p ~/soft-and-wet/{data/navidrome,music,docs,scripts}
cd ~/soft-and-wet
nano compose.yaml
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
printf '.env\ndata/\nmusic/\n' > .gitignore
```

The initial `.gitignore` was later expanded for publication; see the [root ignore rules](../.gitignore). The root folder on the Debian machine (`soft-and-wet`) and the GitHub repository name (`soft-and-wet-homelab`) are intentionally different.

## 4. Start the first container

Place the repository's [compose.yaml](../compose.yaml) in `~/soft-and-wet` and use the generated `.env` from the previous step:

```bash
cd ~/soft-and-wet
sudo docker compose up -d
sudo docker compose ps
sudo docker stats --no-stream
```

The observed startup pulled `navidrome/navidrome:latest`, created network `soft-and-wet_default`, and started a single `navidrome` container.

## 5. Scope of the first installation

The host stayed headless, with no Apache installation or permanent USB drive. The 1.5 GiB swap partition was left as installed. Navidrome was the only application container. Portainer, Uptime Kuma and Heimdall remained candidates.

## Next steps

- [Navidrome deployment](services/navidrome.md)
- [Transfer the music library](music-library.md)
- [Connect Android through Tailscale](services/tailscale.md)

## Official references

- [Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/)
- [Install Navidrome with Docker](https://www.navidrome.org/docs/installation/docker/)
