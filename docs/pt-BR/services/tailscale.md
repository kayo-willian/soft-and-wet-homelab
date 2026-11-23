# Tailscale — acesso remoto implementado

[English](../../services/tailscale.md) · [Serviços](README.md)

## Por que utilizamos

O Tailscale cria uma conexão privada entre o Debian doméstico e o Samsung A34, permitindo ouvir o Navidrome fora de casa sem abrir portas do roteador. **É um serviço do Debian, não um contêiner Docker.**

Site oficial: [Tailscale](https://tailscale.com/) · [Instalação Linux](https://tailscale.com/docs/install/linux)

## Instalação utilizada

~~~bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
tailscale ip -4
~~~

O `tailscale up` apresentou um endereço de autenticação. Debian e celular foram autorizados na mesma tailnet e receberam endereços próprios da rede privada.

Os **endereços reais não são publicados** no repositório. A conta e os tokens de autenticação também permanecem privados.

## Configuração no Android

1. Instalar Tailscale no Samsung A34.
2. Entrar na mesma conta/tailnet do Debian.
3. Ativar a conexão privada no Android.
4. Abrir um cliente musical compatível com Navidrome/Subsonic.
5. Configurar `http://<DEBIAN_TAILSCALE_IP>:4533` com as credenciais do Navidrome.
6. Desligar o Wi-Fi doméstico, usar dados móveis e confirmar reprodução.

**Resultado confirmado:** acesso remoto e streaming de músicas funcionaram. Não há confirmação inequívoca do nome do cliente musical Android utilizado, portanto o documento não afirma ter instalado um aplicativo específico.

## Manutenção e diagnóstico

~~~bash
tailscale status
tailscale ip -4
sudo systemctl status tailscaled
~~~

Se o acesso remoto falhar, conferir os dois equipamentos na tailnet, o contêiner Navidrome e o endereço do servidor. Não redirecionar portas no roteador como solução improvisada.

## Limite de segurança

A criptografia da tailnet protege o tráfego entre os dispositivos. Porém o Navidrome continua publicado pelo Docker em `0.0.0.0:4533` e `[::]:4533`; isso **não restringe** automaticamente o acesso pela LAN. Uma revisão específica ainda está pendente.
