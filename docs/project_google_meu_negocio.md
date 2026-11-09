---
tags: [automacao, n8n, google-meu-negocio, marketing]
atualizado: 2026-11-09
status: em produção
---

# Google Meu Negócio — Post semanal automático (n8n)

Post semanal do Perfil da Empresa no Google (perfil **GG Contabilidade**, Vila Leopoldina) **100% automático, sem revisão**, desde 09/11/2026. Primeira rodada automática: segunda 16/11/2026.

## Como funciona

1. **Segunda 07:45 – Gerador** `GG - Google Meu Negocio - Gerar Post Semanal (Claude)` (id `9jjnvIIy3VvUWNrv`)
   - Lê os temas já publicados na planilha (evita repetir).
   - Chama a API da Anthropic (Claude + pesquisa na web) para escrever o post: 3 parágrafos, "aqui na GG, na Vila Leopoldina", termina em "Fale com a gente.", sem hashtag, link, telefone ou e-mail.
   - Valida: até 1.500 caracteres, sem link/hashtag, não vazio.
   - Grava na planilha com status **Aprovado**. Se falhar → linha com status **Erro** e o motivo.
2. **Dias úteis 10:00 e 16:00 – Publicador** `GG - Google Meu Negocio - Publicar via Sheets` (id `m1lCCAJeWua9Ifh2`)
   - Lê as linhas com status **Aprovado** (1 por vez).
   - Refresh Token Google → conta → local "GG Contabilidade" → cria o post (botão "Saiba mais" → home do site).
   - Marca a linha como **Publicado** (data/hora + Post_id) ou **Erro**.
   - Envia e-mail para rodolfo@ggservicodecontabilidade.com.br (Gmail GG) com assunto "Post publicado no Google — título" e só o texto publicado.
3. **Plano B manual** `GG - Google Meu Negocio - Post Semanal` (id `nF43b0kSw3zKw6K2`) — webhook `/webhook/gmb-post` (campo `caption`, Header Auth).

## Planilha

- Google Planilhas **"Postagem Google"**: https://docs.google.com/spreadsheets/d/16sgnS4uAefuvjIMNyz4KDl6Y9Ocxi128qMGB_YBiJcA
- Aba `Planilha1` · colunas: `Data_post | titulo | texto | status | publicado_em | Post_id | erro`
- Status que o n8n entende: **Aprovado** (exatamente assim) → publica; **Publicado** / **Erro** são gravados pelo n8n.
- Credencial no n8n: "Google Sheets account 2" (a planilha precisa estar nessa conta ou compartilhada com ela como Editor).
- Precisa ser **Planilha Google nativa** (um .xlsx aberto no Drive não funciona). Cópia do arquivo antigo também em `\\Gg_servidor\gg_documentos\GG SERVICOS EMPRESARIAIS\GERENCIA DE VENDAS\GOOGLE MEU NEGOCIO\`.

## Histórico

- 02/11/2026 — Fim da escala 6x1 (publicado manualmente pelo painel)
- 04/11/2026 — Workflow webhook publicado após aprovação da API do Google
- 09/11/2026 — 13º salário: prazos das parcelas — 1º post publicado via Sheets (14h46)
- 09/11/2026 — Gerador automático e e-mail de confirmação em produção

## O que mudou em 09/11/2026

- Tarefa agendada do Claude "Post Google Meu Negócio — GG (segunda)" **desativada** (substituída pelo gerador do n8n).
- Claude Doc "Posts Google Meu Negócio — GG" deixa de receber posts novos — o registro passa a ser a planilha.
- Fluxos registrados no `GG ORQUESTRADOR\manifest.yaml`, seção `workflows_n8n` (só documentação; o script monitora apenas o Task Scheduler).

## Cuidados / pendências

- Sem revisão humana: erro de prazo ou dado vai ao ar. No teste de 09/11 o texto gerado trouxe uma data de DCTFWeb divergente do calendário da GG — vale olhar a planilha nas segundas.
- Edições em workflow ativo: salvar e clicar **Publish** de novo no n8n (senão a versão em produção continua a antiga).
- Próximo passo possível: orquestrador monitorar execuções do n8n via API (exige chave de API do n8n + ajuste no `gg_orquestrador.py`).
