# Solução de problemas, alternativas e decisões

[English](../troubleshooting.md) · [Documentação](README.md) · [Início](../../README.pt-BR.md)

Este registro diferencia **problemas encontrados**, **alternativas discutidas**, **verificações bem-sucedidas** e **riscos pendentes**. Só algumas etapas têm saídas do terminal registradas.

## A. Problemas realmente encontrados

### T01 — `swapon`: comando não encontrado

- **Observado:** o comando `swapon` não era encontrado pelo nome curto no shell da conta comum.
- **Solução usada:** `sudo /usr/sbin/swapon --show`, que funcionou.
- **Resultado:** `/dev/mmcblk2p3` é uma **partição swap de 1,5 GiB**, com 0 de uso naquele momento.
- **Alternativas avaliadas:** aumentar a swap total para 4 GB, criar arquivo de 2,5 GB, usar zram.
- **Decisão:** **não** redimensionar partições nem criar swapfile/zram antes de medir a carga real.

```bash
sudo /usr/sbin/swapon --show
cat /proc/swaps
free -h
```

### T02 — `Unknown Album` no Navidrome

- **Observado:** diversas faixas exibiam capa, título/artista, mas o cartão do álbum mostrava `Unknown Album`.
- **Causa:** campo `album` ausente ou incompleto nos metadados internos. Renomear MP3/FLAC não altera automaticamente essa informação.
- **Tentativa:** MusicBrainz Picard, com discussão sobre a função Scan, a edição de metadados e o reenvio por `rsync`.
- **Resultado:** Picard **não resolveu** a apresentação na sessão observada.
- **Alternativa sugerida:** colocar o nome de cada música no campo álbum. **Descartada** porque confundiria faixas com álbuns reais.
- **Solução escolhida:** usar a seção **Músicas (Songs)** do Navidrome e preservar as informações não verificadas.

### T03 — Aplicativo Navidrome no Android

- **Observado:** o repositório principal do Navidrome não distribui um APK oficial próprio.
- **Opções avaliadas:** clientes Subsonic/OpenSubsonic, incluindo Symfonium, Substreamer, Ultrasonic, Navis e Chora.
- **Resultado:** confirmei depois que um aplicativo compatível funcionou no Samsung A34, inclusive fora de casa.
- **Limite da evidência:** não ficou comprovado qual aplicativo de terceiros foi instalado, então a documentação não atribui um cliente específico.
- **Configuração:** servidor no IP Tailscale do Debian, porta 4533; fora do Wi-Fi doméstico, não usar o IP da LAN.

### T04 — Playlist do Spotify e arquivos locais

- **Objetivo inicial:** ouvir no Navidrome músicas de uma playlist pessoal do Spotify.
- **Limitação:** arquivos offline do Spotify Premium não são MP3/FLAC comuns transferíveis.
- **Opções discutidas:** compra ou download permitido de arquivos, ferramentas compatíveis com fontes autorizadas, biblioteca selecionada manualmente.
- **Decisão:** escolher dez músicas da playlist **Brasil**, obter arquivos locais e transferi-los para o Debian. Não houve auditoria da origem/licença de cada arquivo; nenhum áudio foi publicado.

### T05 — Pasta, álbum e playlist são coisas diferentes

- **Observado:** a pasta `music/Brasil/` contém as dez faixas selecionadas, mas a tela de álbuns não a interpreta automaticamente como um único álbum.
- **Explicação:** o Navidrome prioriza os metadados das faixas, não apenas o nome da pasta.
- **Decisão:** usar a seção **Músicas**. Uma playlist dentro do Navidrome pode ser criada separadamente; não afirmamos que houve exportação ou automação de playlists.

## B. Alternativas discutidas (não são falhas de implantação)

| Tema | Possibilidade | Decisão | Motivo |
| --- | --- | --- | --- |
| Armazenamento | Pendrive Ventoy (~51 GB livres) | **Descartado** para uso permanente | Não deixar mídia removível conectada |
| Armazenamento | eMMC interno | **Em uso** | Simplicidade, embora seja pequeno e sem redundância |
| Swap | Aumentar partição de 1,5 GiB para 4 GB | **Não feito** | Sem pressão de memória observada. Redimensionar a partição traz riscos |
| Swap | Arquivo extra ou zram | **Não feito** | Reavaliar após medir o servidor |
| Servidor web | Apache pelo instalador Debian | **Não instalado** | Sem necessidade atual; possível estudo futuro |
| Mídia | Plex / Jellyfin | **Adiados** | Navidrome atende à biblioteca musical |
| Downloads | Sonarr/Radarr e automação de filmes | **Fora do escopo** | Evitar consumo de armazenamento |
| Acesso externo | Redirecionar portas no roteador | **Evitado** | O Tailscale permitiu o acesso sem encaminhar portas |
| Acesso externo | Tailscale instalado no host | **Implementado** | Streaming remoto confirmado |
| Docker | Portainer | **Não instalado** | Próximo serviço possível |
| Monitoramento | Uptime Kuma / Beszel | **Não instalado** | Avaliar consumo antes |
| Logs | Dozzle | **Não instalado** | Conveniência opcional |
| Painel | Heimdall | **Não instalado** | Conveniência opcional |
| Arquivos | File Browser | **Não instalado** | Projeto original arquivado |
| Backup | Copiar para outro computador | **Não feito** | Plano sem teste de restauração |
| Energia | Ignorar tampa fechada via logind | **Sugerido, não verificado** | Configuração persistente não confirmada |

## C. Diagnóstico reproduzível

### Navidrome não abre

```bash
cd ~/soft-and-wet
sudo docker compose ps
sudo docker compose logs --tail=100 navidrome
sudo docker stats --no-stream
```

Na LAN, acessar `http://<DEBIAN_LAN_IP>:4533`; remotamente, `http://<DEBIAN_TAILSCALE_IP>:4533`. Não redirecionar a porta 4533 no roteador.

### Músicas não aparecem

```bash
cd ~/soft-and-wet
ls -lh music/Brasil/
du -sh music
sudo docker compose logs --tail=100 navidrome
```

Conferir permissões conforme UID/GID, formato e indexação; o Compose usa `ND_SCANSCHEDULE: 1h`.

### Duplicatas depois de renomear

O `rsync -avh` atualiza caminhos iguais, mas **não remove** arquivos antigos com nomes diferentes. Conferir a pasta remota antes de apagar qualquer coisa. Evitar `--delete` sem revisão.

### Pouco armazenamento ou memória

```bash
df -h /
free -h
sudo docker system df
sudo docker stats --no-stream
sudo /usr/sbin/swapon --show
```

Os 32,51 MiB de RAM foram medidos **na inicialização** do Navidrome, não sob carga constante.

### Problemas com SSH/Tailscale

```bash
sudo systemctl status ssh docker tailscaled --no-pager
tailscale status
tailscale ip -4
hostname -I
```

Não presumir que o hostname `debian` resolva automaticamente em toda rede. Usar o endereço correto quando necessário.

## D. Riscos técnicos pendentes

1. Porta do Navidrome publicada em `0.0.0.0:4533` e `[::]:4533`; Tailscale não restringe isso sozinho.
2. Não foi comprovado teste de backup e restauração dos dados.
3. Sem medições de RAM, CPU ou espaço livre em regime estável após a inclusão das dez faixas.
4. SSH apenas por chave, configuração de UFW/Fail2ban, regras Docker e fixação de versão da imagem não verificados.
5. Sem scripts automatizados ou outros contêineres instalados.

Ver [segurança](seguranca.md) e [operações](operacoes.md). Comandos recomendados para mitigação não são apresentados como alterações já executadas.
