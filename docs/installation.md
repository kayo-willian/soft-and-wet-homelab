# Installation and preparation

> **Evidence boundary:** The steps below reconstruct the setup instructions used for this project. The user confirmed installation of the tools and Docker, but a complete original shell history was not preserved. Commands explicitly observed in the terminal are distinguished in the verification section.

## 1. Debian 13 base system

Install Debian 13 (Trixie) on the Positivo notebook without a desktop environment. Enable an SSH server and standard system utilities during setup, or install OpenSSH afterward. The installed host is named `debian` and is operated in another room over the LAN.

The initial package set was provided as:

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

## 2. Docker Engine and Compose plugin

The previously supplied Docker installation procedure used the official Debian package repository:

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

The following checks were observed later in the user's terminal:

```bash
sudo docker system df
sudo docker compose version
```

The actual reported Compose version was `v5.8.0`. Docker was nearly empty at the first measurement: no running containers, no local volumes, and one tiny image. Installing the Compose plugin in the future may yield a newer version.

## 3. Project directory

The actual commands used for the first application were:

```bash
mkdir -p ~/soft-and-wet/{data/navidrome,music,docs,scripts}
cd ~/soft-and-wet
nano compose.yaml
printf 'HOST_UID=%s\nHOST_GID=%s\n' "$(id -u)" "$(id -g)" > .env
printf '.env\ndata/\nmusic/\n' > .gitignore
```

The initial `.gitignore` was later expanded for publication; see the top-level repository file. The root folder on the Debian machine (`soft-and-wet`) and the GitHub repository name (`soft-and-wet-homelab`) are intentionally different.

## 4. Start the first container

Place the repository's `compose.yaml` in `~/soft-and-wet` and use the generated `.env` from the previous step:

```bash
cd ~/soft-and-wet
sudo docker compose up -d
sudo docker compose ps
sudo docker stats --no-stream
```

The observed startup pulled `navidrome/navidrome:latest`, created network `soft-and-wet_default`, and started a single `navidrome` container.

## 5. Next reading

- [Navidrome deployment](services/navidrome.md)
- [Music transfer](music-library.md)
- [Tailscale](services/tailscale.md)

References: [Docker on Debian](https://docs.docker.com/engine/install/debian/) and [Navidrome Docker installation](https://www.navidrome.org/docs/installation/docker/).
