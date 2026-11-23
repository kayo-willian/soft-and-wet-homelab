# Redes e acesso remoto

[English](../networking.md) · [Índice](README.md)

## Topologia confirmada

O Debian e o computador Ubuntu estão na mesma rede doméstica. O acesso de administração é por **SSH**, e o Navidrome atende por HTTP em `http://<DEBIAN_LAN_IP>:4533`.

O celular usa **Tailscale** e conecta ao Navidrome em `http://<DEBIAN_TAILSCALE_IP>:4533` quando está fora da rede local. Não são publicados endereços IP privados reais.

## Endereços e interfaces

O comando `hostname -I` mostrou endereços da LAN, pontes Docker e IPv6. A rede do Compose foi criada como `soft-and-wet_default`.

A saída do `sudo docker compose ps` continha:

~~~text
navidrome ... Up ... 0.0.0.0:4533->4533/tcp, [::]:4533->4533/tcp
~~~

**Interpretação:** o Docker publicou a porta 4533 em todas as interfaces IPv4 e IPv6 do host. Isso **não prova** acesso pela internet pública, que depende de roteador, NAT e firewall. Não houve configuração de encaminhamento de portas no roteador.

## Protocolos

| Tecnologia | Função |
|---|---|
| SSH | Administração headless e transferência `rsync` |
| HTTP/TCP 4533 | Interface e API do Navidrome |
| Tailscale | Rede privada criptografada entre Debian e celular |
| Docker bridge | Rede do contêiner e publicação de porta |

## Comandos de diagnóstico

~~~bash
hostname -I
ip -br address
sudo docker network ls
sudo docker compose ps
tailscale status
tailscale ip -4
~~~

## Pendência de segurança

Portas publicadas pelo Docker podem interagir com UFW de forma diferente das regras comuns do host. Não basta instalar UFW para presumir que o Navidrome está restrito ao Tailscale. Essa revisão ainda não foi executada.

Referências: [Docker port publishing](https://docs.docker.com/engine/network/port-publishing/) e [Tailscale IPs](https://tailscale.com/docs/concepts/ip-and-dns-addresses).
