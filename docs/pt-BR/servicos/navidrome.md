# Navidrome — serviço implementado

[English](../../services/navidrome.md) · [Documentação](../README.md) · [Início](../../../README.pt-BR.md)

## Objetivo

O Navidrome é a primeira aplicação útil do Soft & Wet: organiza uma biblioteca musical local e disponibiliza reprodução pelo navegador e por clientes Android compatíveis. **Não baixa músicas automaticamente**. Comecei com dez faixas escolhidas manualmente para evitar crescimento descontrolado do armazenamento.

## Docker Compose original

A configuração utilizada na instalação permanece no arquivo raiz [`compose.yaml`](../../../compose.yaml):

```yaml
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
```

O arquivo `.env`, não versionado, recebe `HOST_UID` e `HOST_GID` obtidos com `id -u` e `id -g`, reduzindo a necessidade de gravar dados da aplicação como root.

## Persistência

| Pasta no Debian | Dentro do contêiner | Finalidade |
| --- | --- | --- |
| `./data/navidrome` | `/data` | Banco e cache; leitura/escrita |
| `./music` | `/music` | Músicas; somente leitura |

Os arquivos precisam ser legíveis pelo usuário configurado; `data/navidrome` também deve permitir escrita. As músicas e o banco de dados ficam fora do Git.

## O que foi testado

1. `sudo docker compose up -d` baixou a imagem e criou o contêiner.
2. `sudo docker compose ps` mostrou `navidrome` ativo com a porta TCP 4533 publicada em `0.0.0.0` e `[::]`.
3. O navegador exibiu a tela inicial **Create Admin**.
4. Configurei a conta administrativa. As dez músicas transferidas apareceram na biblioteca.
5. Parte dos arquivos mostrou `Unknown Album`, mas os títulos apareceram corretamente na seção **Músicas**. Mantive os metadados de álbum sem alterações.
6. A reprodução remota pelo celular funcionou após configurar Tailscale.

As credenciais de administração são privadas. Confirmei a reprodução no celular separadamente do registro do terminal e não anotei o nome do cliente.

## Transferir músicas do Ubuntu

No Ubuntu, substitua a conta e o IP de exemplo abaixo pelo login e pelo endereço do Debian na LAN. O caminho remoto parte da pasta pessoal dessa conta. As duas máquinas precisam de `rsync`.

```bash
DEBIAN_USER=homelab
DEBIAN_HOST=192.0.2.10
rsync -avh --progress "$HOME/Músicas/Brasil/" \
  "${DEBIAN_USER}@${DEBIAN_HOST}:soft-and-wet/music/Brasil/"
```

O Compose utiliza `ND_SCANSCHEDULE: 1h`; as músicas devem ser incorporadas após varredura. Se a versão instalada oferecer essa opção, também é possível iniciar uma varredura pela interface. Confirmar os resultados na interface e consultar os logs se necessário.

## Comandos úteis

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker stats --no-stream navidrome
du -sh ~/soft-and-wet/music ~/soft-and-wet/data/navidrome
df -h /
```

A opção `docker compose down -v` pode remover volumes nomeados do projeto e volumes anônimos anexados. Nesta configuração, a biblioteca e o banco usam pastas do host (bind mounts).

## Estado e limitações

**Instalado e testado.** Ainda faltam backup/restauração independente, medições prolongadas e restrição de publicação da porta a interfaces selecionadas. Não há biblioteca extensa nem outros contêineres de aplicação.

## Referências oficiais

- [Navidrome](https://www.navidrome.org/)
- [Instalação com Docker](https://www.navidrome.org/docs/installation/docker/)
