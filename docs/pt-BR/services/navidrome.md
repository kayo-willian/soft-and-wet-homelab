# Navidrome — serviço implementado

[English](../../services/navidrome.md) · [Serviços](README.md)

## Objetivo

O Navidrome é a primeira aplicação útil do Soft & Wet: organiza uma biblioteca musical local e disponibiliza reprodução pelo navegador e por clientes Android compatíveis. **Não baixa músicas automaticamente**. Começamos com dez faixas escolhidas manualmente para evitar crescimento descontrolado do armazenamento.

Site oficial: [Navidrome](https://www.navidrome.org/) · [Documentação Docker](https://www.navidrome.org/docs/installation/docker/)

## Docker Compose original

A configuração utilizada na instalação permanece no arquivo raiz [`compose.yaml`](../../../compose.yaml):

~~~yaml
services:
  navidrome:
    image: navidrome/navidrome:latest
    container_name: navidrome
    user: "${HOST_UID}:${HOST_GID}"
    ports:
      - "4533:4533"
    restart: unless-stopped
    volumes:
      - ./data/navidrome:/data
      - ./music:/music:ro
    environment:
      ND_LOGLEVEL: info
      ND_SCANSCHEDULE: 1h
    logging:
      driver: json-file
      options:
        max-size: "5m"
        max-file: "2"
~~~

O arquivo `.env`, não versionado, recebe `HOST_UID` e `HOST_GID` obtidos com `id -u` e `id -g`, reduzindo a necessidade de gravar dados da aplicação como root.

## Persistência

| Pasta no Debian | Dentro do contêiner | Finalidade |
|---|---|---|
| `./data/navidrome` | `/data` | Banco e cache; leitura/escrita |
| `./music` | `/music:ro` | Músicas; somente leitura |

Os arquivos precisam ser legíveis pelo usuário configurado; `data/navidrome` também deve permitir escrita. Nenhuma música ou banco é publicada no GitHub.

## O que foi testado

1. `sudo docker compose up -d` baixou a imagem e criou o contêiner.
2. `sudo docker compose ps` mostrou `navidrome` ativo na porta 4533, publicado em todas as interfaces.
3. O navegador exibiu a tela inicial **Create Admin**.
4. A conta administrativa foi configurada e dez músicas foram transferidas.
5. Parte dos arquivos mostrou `Unknown Album`, mas os títulos apareceram corretamente na seção **Músicas**.
6. A reprodução remota pelo celular funcionou após configurar Tailscale.

Não há registro público da senha do administrador. O aplicativo Android exato não foi comprovado de forma independente.

## Transferir músicas do Ubuntu

~~~bash
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  kayo@<DEBIAN_LAN_IP>:/home/kayo/soft-and-wet/music/Brasil/
~~~

O Compose utiliza `ND_SCANSCHEDULE: 1h`; as músicas devem ser incorporadas após varredura. Confirmar os resultados na interface e consultar os logs se necessário.

## Comandos úteis

~~~bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker stats --no-stream navidrome
du -sh ~/soft-and-wet/music ~/soft-and-wet/data/navidrome
df -h /
~~~

## Estado e limitações

**Instalado e testado.** Ainda faltam backup/restauração independente, medições prolongadas e restrição de publicação da porta a interfaces selecionadas. Não há biblioteca extensa nem outros contêineres de aplicação.
