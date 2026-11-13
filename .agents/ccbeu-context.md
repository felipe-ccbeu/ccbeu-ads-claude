# Contexto CCBEU para Ads

> Lido por todas as skills `ccbeu-ads-*`. O que estiver como `PREENCHER` ainda não foi definido:
> as skills tratam como `unknown` e perguntam, em vez de chutar.

## Produto
- Marca: CCBEU
- O que é / o que entrega: PREENCHER
- Se houver mais de um produto/plano, listar cada um abaixo (todo relatório separa campanha por produto,
  porque preço, garantia e público costumam ser diferentes).
- Cliente-alvo (ICP): PREENCHER
- Tom de voz / o que nunca dizer: PREENCHER

### Produto / plano 1: PREENCHER
- Foco: PREENCHER
- O que entrega: PREENCHER
- Diferencial: PREENCHER
- Preço: PREENCHER
- Garantia: PREENCHER
- Dor principal: PREENCHER

### Resumo estratégico
- PREENCHER (papel de cada produto: entrada, principal, upsell...)
- Ao analisar criativo/copy, checar se o anúncio promete o que o produto dele entrega.

## Conta Meta
- IDs ficam no `.env` (não aqui). Confirmar a cada rodada com `whoami`.
- Fuso dos relatórios: PREENCHER (ex.: America/Sao_Paulo, -0300)
- Início dos anúncios (primeira campanha): PREENCHER

## Funil e conversão
Etapas (ajustar ao produto real):
Impressões → Cliques no link → Visita à página (LPV) → Conversão principal → PREENCHER (etapa depois da conversão)

- Conversão principal (evento de otimização do pixel): PREENCHER (ex.: `lead`, `purchase`, `complete_registration`)
- Como o resultado é medido depois do clique (CRM, planilha, painel): PREENCHER
- Landing page / destino dos anúncios: PREENCHER

## Metas de custo
- Custo por conversão — limite aceitável: PREENCHER
- Custo por conversão — meta ideal: PREENCHER
- Orçamento total de todas as campanhas (mensal e diário): PREENCHER
- Regra de escala quando um anúncio/conjunto estiver indo muito bem: PREENCHER
- Valor de um cliente fechado (LTV/CAC), se existir: PREENCHER

## Regras de negócio
- Todos os anúncios nascem PAUSED. Ativar só com ordem explícita do usuário.
- Teto de orçamento diário: variável `MAX_DAILY_BUDGET` do `.env`.
