---
name: ccbeu-ads-report
description: Coleta métricas reais dos anúncios Meta Ads do CCBEU via MCP (SOMENTE LEITURA), gera relatório HTML semanal por conjunto com performance por anúncio, distribuição de budget e análise estratégica (deu certo / deu errado / dá pra testar / não dá pra testar). Use para "relatório de ads", "métricas da semana" ou /ccbeu-ads-report.
---

# ccbeu-ads-report

Relatório semanal completo. **Somente leitura**: não altera nada na conta e não envia o relatório para
nenhum sistema externo a menos que o usuário peça depois.
Ferramentas: `whoami`, `get_account_balance`, `get_campaigns`, `get_adsets`, `list_ads`, `get_ad`,
`get_campaign_insights`, `get_ad_insights`.

## Contexto
Ler `.agents/ccbeu-context.md`. IDs descobertos via MCP a cada rodada. Metas ausentes = "não definido".

## Passo 1: Coletar
- `whoami`, `get_account_balance` (status ≠ 1 vai no topo, em destaque).
- `get_campaigns` e `get_adsets` (status, otimização, orçamento diário, início).
- `list_ads` por conjunto (status efetivo, copy, CTA).
- Janela principal: últimos 7 dias completos; comparar com os 7 anteriores.
- `get_ad_insights` de cada anúncio (impressões, cliques, gasto, CTR, CPC, alcance, conversões).

## Passo 2: Por conjunto
Tabela: gasto, impressões, CTR, CPC, conversões, custo por conversão, frequência. Dentro dele, tabela por anúncio com as
mesmas colunas. Anúncios com menos de 1.000 impressões ou 0 conversão: marcar "amostra pequena", sem ranking.

## Passo 3: Distribuição de budget
Percentual do gasto por conjunto e por anúncio. Sinalizar concentração (> 60% num só anúncio) e conjuntos ativos sem gasto.

## Passo 4: Análise estratégica (4 blocos, só com evidência)
- **Deu certo:** o que melhorou/funcionou, com número e janela.
- **Deu errado:** o que piorou ou não entregou, com número e janela.
- **Dá pra testar:** hipóteses testáveis (mecanismo, métrica de sucesso, janela, condição de reversão).
- **Não dá pra testar:** o que falta de dado/volume para concluir e o que seria necessário.
Nunca recomendar pausar por custo médio; sempre hipótese. Cite o n.

## Passo 5: Saída
HTML (CSS puro, entidades HTML para acentos) em `outputs/ads/report-<YYYY-MM-DD>.html` e o JSON dos dados
ao lado. No chat: resumo de 5 linhas e o caminho do arquivo. Nada é enviado a sistema externo.
