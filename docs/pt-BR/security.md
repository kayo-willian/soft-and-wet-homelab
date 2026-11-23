# Segurança e limites de publicação

[English](../security.md) · [Índice](README.md)

## Implementado ou diretamente observado

- Debian sem interface gráfica, administrado por SSH.
- Navidrome em contêiner com UID/GID do host definidos no `.env` não versionado.
- Biblioteca `/music` montada no contêiner como **somente leitura**.
- `.gitignore` exclui mídia pessoal, banco de dados, cache, senhas e arquivos locais.
- Tailscale conecta Debian e Android pela rede privada.
- O acesso remoto não exigiu encaminhamento público da porta no roteador.

## Não comprovado ou pendente

- Autenticação SSH exclusivamente por chaves e desativação de senhas.
- Regras UFW ativas, estado dos jails do Fail2ban e política de firewall específica do Docker.
- Restrição da porta publicada do Navidrome a interfaces selecionadas.
- HTTPS diretamente no serviço HTTP local.
- Backup independente e teste de restauração.
- Fixação da imagem Navidrome por digest/versão e monitoramento de vulnerabilidades.

**Atenção:** o `compose.yaml` atual usa `4533:4533`, publicando a porta em todas as interfaces do host. Uma conexão Tailscale funcionando **não elimina** essa exposição na LAN. Qualquer ajuste futuro precisa ser aplicado e testado no servidor antes de ser marcado como concluído.

## Regras para publicar no GitHub

Deixar fora do repositório:
- `.env`, senhas, tokens, chaves privadas e dados da conta Tailscale.
- `data/`, `music/`, `secrets/`, `backups/`, `local/` e bancos SQLite.
- Listas reais de endereços e informações sensíveis da rede.
- Arquivos MP3/FLAC e logs brutos.

Antes de publicar via Git:

~~~bash
git status --short
git diff --cached --name-only
~~~

Ao enviar arquivos pela interface web, selecionar apenas documentos e configurações publicáveis — o `.gitignore` não substitui essa revisão manual.

O Soft & Wet é um laboratório doméstico, não um serviço de produção com controles de segurança formalmente validados.
