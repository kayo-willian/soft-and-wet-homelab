# Segurança e limites de publicação

[English](../security.md) · [Documentação](README.md) · [Início](../../README.pt-BR.md)

## Implementado ou diretamente observado

- Debian sem interface gráfica, administrado por SSH.
- Navidrome em contêiner com UID/GID do host definidos no `.env` não versionado.
- Biblioteca `/music` montada no contêiner como **somente leitura**.
- As [regras de exclusão](../../.gitignore) cobrem diretórios locais de mídia e dados, `.env`, arquivos de chave e logs. Confira também o conteúdo preparado para o commit.
- Tailscale conecta Debian e Android pela rede privada.
- O acesso remoto não exigiu encaminhamento público da porta no roteador.

## Não comprovado ou pendente

- Autenticação SSH exclusivamente por chaves e desativação de senhas.
- Regras UFW ativas, estado dos jails do Fail2ban e política de firewall específica do Docker.
- Restrição da porta publicada do Navidrome a interfaces selecionadas.
- HTTPS diretamente no serviço HTTP local.
- Backup independente e teste de restauração.
- Fixação da imagem Navidrome por digest/versão e monitoramento de vulnerabilidades.

O [arquivo Compose](../../compose.yaml) publica a porta TCP 4533 em todas as interfaces do host. O Tailscale criptografa o tráfego remoto. A porta publicada pode continuar acessível pela LAN. O Docker pode encaminhar tráfego ao contêiner antes da aplicação das regras usuais de entrada do UFW. A restrição de interfaces e as regras de firewall do Docker ainda precisam de um teste específico.

## Regras para publicar no GitHub

Deixar fora do repositório:

- `.env`, senhas, tokens de API, chaves privadas e dados da conta Tailscale.
- `data/`, `music/`, `secrets/`, `backups/`, `local/` e arquivos de bancos de dados, incluindo SQLite.
- Nomes de contas locais, listas reais de dispositivos e endereços IP e informações sensíveis da rede.
- Arquivos MP3/FLAC e logs brutos.

Antes de publicar via Git:

```bash
git status --short
git diff --cached --name-only
```

Ao enviar arquivos pela interface web, selecionar apenas documentos e configurações publicáveis — o `.gitignore` não substitui essa revisão manual.

O Soft & Wet é um laboratório doméstico, não um serviço de produção com controles de segurança formalmente validados.
