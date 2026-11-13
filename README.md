# ccbeu-ads-claude

Automações com Claude Code — agentes, tarefas agendadas e scripts recorrentes.

## Estrutura

```
.env                      # credenciais (fora do git)
.env.example              # modelo do .env
.mcp.json                 # MCP ccbeu-meta-ads, carrega o .env via node --env-file
CLAUDE.md                 # regras do projeto
.agents/ccbeu-context.md   # produto, funil e metas (preencher os PREENCHER)
.claude/skills/
  ccbeu-ads-audit/        # auditoria profunda, só leitura
  ccbeu-ads-pixel-check/  # alerta diário de conta e rastreamento, só leitura
  ccbeu-ads-onepager/     # resumo semanal de uma página, só leitura
  ccbeu-ads-report/       # relatório semanal completo, só leitura
  ccbeu-ad-management/    # única que escreve, só com pedido explícito
outputs/ads/              # saídas geradas (fora do git)
```

## Como usar

> Instalando em outro computador? Siga o passo a passo completo em [SETUP.md](SETUP.md).

1. Preencha o `.env` (veja abaixo) e os campos `PREENCHER` de `.agents/ccbeu-context.md`.
2. Abra o Claude Code nesta pasta e aprove o servidor `ccbeu-meta-ads` quando ele pedir.
3. Teste: peça `whoami`. Deve voltar produto, conta e Página.
4. Rode `/ccbeu-ads-audit` para a primeira auditoria.

O MCP (pasta `meta-ads-mcp`) não lê `.env` sozinho. O `.mcp.json` usa
`node --env-file=<caminho>\.env`, então os segredos ficam só no `.env`.
O `.mcp.json` fica fora do git: cada pessoa cria o seu a partir do `.mcp.json.example`.

## Credenciais do Meta

| Variável | Onde achar |
|---|---|
| `META_ACCESS_TOKEN` | https://business.facebook.com/settings → Usuários → Usuários do sistema → Gerar token (app + `ads_read`; `ads_management` só para criar/editar) |
| `META_AD_ACCOUNT_ID` | https://business.facebook.com/settings → Contas → Contas de anúncios (número com prefixo `act_`) |
| `META_PAGE_ID` | https://business.facebook.com/settings → Contas → Páginas |
| `META_PIXEL_ID` | https://business.facebook.com/settings → Fontes de dados → Pixels |

Apps: https://developers.facebook.com/apps · Docs: https://developers.facebook.com/docs/marketing-apis ·
Testar token: https://developers.facebook.com/tools/debug/accesstoken

## Stack

- [Claude Code](https://claude.ai/code)
- claude-sonnet-4-6
