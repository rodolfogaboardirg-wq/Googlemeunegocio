# Google Meu Negócio — GG Contabilidade

Automação do post semanal do Perfil da Empresa no Google (perfil **GG Contabilidade**, Vila Leopoldina/SP), rodando no n8n da GG (Hostinger).

| Etapa | Workflow n8n | Quando |
|---|---|---|
| Gerar post (Claude + pesquisa web) e gravar como Aprovado | `GG - Google Meu Negocio - Gerar Post Semanal (Claude)` | seg 07:45 |
| Publicar aprovados, marcar Publicado/Erro e enviar e-mail | `GG - Google Meu Negocio - Publicar via Sheets` | dias úteis 10:00 e 16:00 |
| Publicação manual (plano B) | `GG - Google Meu Negocio - Post Semanal` (webhook `/webhook/gmb-post`) | sob demanda |

Planilha de controle: Google Planilhas "Postagem Google" (colunas `Data_post | titulo | texto | status | publicado_em | Post_id | erro`).

## Pastas

- `docs/` — documentação completa (mesma nota do Obsidian).
- `n8n/` — exports dos workflows **sem segredos**: o nó "Refresh Token Google" vai sem client_id/client_secret/refresh_token e as credenciais aparecem só pelo nome. Ao importar, preencher no n8n.
- `orquestrador/` — cópia do `manifest.yaml` do GG ORQUESTRADOR (seção `workflows_n8n`).

> Nunca commitar tokens, senhas, chaves de API ou o `.env` do orquestrador.
