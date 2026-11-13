---
name: ccbeu-ads-onepager
description: Gera um resumo executivo de UMA PÁGINA, SOMENTE LEITURA, do funil de Meta Ads do CCBEU (impressões → clique → visita → conversão), com custo por etapa, KPIs com variação semanal, anúncio destaque, risco e recomendação. Complementa o relatório longo ccbeu-ads-report. Use para "resumo dos ads", "como está o funil" ou /ccbeu-ads-onepager. Cadência sugerida: semanal.
---

# ccbeu-ads-onepager

Resumo lido em ~60 segundos, com o funil como peça central. **Somente leitura** (`whoami`,
`get_account_balance`, `get_campaigns`, `get_adsets`, `list_ads`, `get_campaign_insights`, `get_ad_insights`).
Não confundir com `ccbeu-ads-report` (ranking completo, distribuição de budget) nem com
`ccbeu-ads-audit` (diagnóstico profundo com propostas).

## Contexto
Ler `.agents/ccbeu-context.md` (funil, conversão principal, metas). Campos `PREENCHER` ficam como
"não definido" no resumo, sem chute. IDs sempre descobertos via MCP, nunca fixos.

## Passo 1: Dados
- `whoami` e `get_account_balance` (status da conta; ≠ 1 vira o primeiro item de risco).
- Janela atual: últimos 7 dias completos. Janela anterior: os 7 dias antes. Calcular variação %.
- `get_campaign_insights` e `get_ad_insights` por anúncio (janela atual) para o anúncio destaque.
- Etapas do funil: impressões → cliques no link → visita à página → conversão principal → etapas seguintes
  do contexto (se existirem e houver fonte).

## Passo 2: Cálculos
Por etapa: volume, taxa de passagem e custo por etapa. KPIs: gasto, impressões, CTR (link), CPC (link),
custo por conversão, conversões. Cada KPI com variação vs. semana anterior. Se algum denominador for 0, mostrar
"—", nunca 0% nem infinito. Amostra < 30 conversões: marcar "preliminar".

## Passo 3: Conteúdo da página
1. Cabeçalho: produto, período, selo "somente leitura".
2. Funil visual com taxa e custo entre as etapas.
3. KPIs com variação semanal.
4. Anúncio destaque (melhor custo por conversão com n mínimo declarado; se n for pequeno, dizer que é indicativo).
5. UM risco (o mais grave: conta, rastreamento, entrega, fadiga) e UMA recomendação, formulada como
   hipótese testável. Nunca "pausar" por custo médio.
6. Rodapé: janelas, fonte (Meta Ads API) e "nada foi alterado".

## Passo 4: Saída
HTML de uma página, CSS puro, responsivo, entidades HTML para acentos. Salvar em
`outputs/ads/onepager-<YYYY-MM-DD>.html`. No chat: 3 linhas com o número principal, o risco e o caminho do arquivo.
