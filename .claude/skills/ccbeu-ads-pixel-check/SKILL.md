---
name: ccbeu-ads-pixel-check
description: >
  Checagem diária SOMENTE LEITURA da saúde do rastreamento de conversão e da conta dos
  anúncios Meta Ads do CCBEU. Confirma se há conversões contadas nas últimas 24h quando
  há gasto, e se a conta está ativa (status e pagamento). Alerta gasto sem conversão, conta
  com status diferente de 1 e conjuntos antigos ainda ativos. Grava em
  outputs/ads/pixel-checks/<data>.log. Use diariamente ou em /ccbeu-ads-pixel-check.
---

# ccbeu-ads-pixel-check

Checagem diária e curta. **Somente leitura**: nunca pausa, ativa nem altera nada; só sinaliza.
Ferramentas permitidas (`ccbeu-meta-ads`): `whoami`, `get_account_balance`, `get_campaigns`,
`get_adsets`, `get_campaign_insights`, `get_ad_insights`, `list_pixels`.

## Contexto
Ler `.agents/ccbeu-context.md` para saber qual é a conversão principal. **Sem IDs fixos**: descobrir tudo
a cada execução. Se a conversão principal estiver `PREENCHER`, usar qualquer action de lead/compra/cadastro/
contato/mensagem e avisar que a definição não foi feita.

## Passo 1: Conta
`whoami` (produto, conta, Página) e `get_account_balance`:
- `account_status` = 1 é o normal. Qualquer outro valor → veredito de alerta de conta, em destaque
  (explica entrega zerada mesmo com campanha ACTIVE).
- Limite de gastos: calcular o que resta e quantos dias isso cobre ao orçamento atual.

## Passo 2: Estrutura ativa
`get_campaigns` e `get_adsets` de cada campanha ativa. Listar conjuntos `ACTIVE` agora.
Se houver mais de um conjunto ativo na mesma campanha, sinalizar (possível gasto duplicado), sem agir.

## Passo 3: Insights das últimas 24h
`get_campaign_insights` (dia anterior completo; se der, também 7 dias para tendência) e extrair:
- `spend`.
- `actions` relacionadas a conversão (o nome varia: `lead`, `onsite_conversion.lead`, `purchase`,
  `complete_registration`, `contact`, `onsite_conversion.messaging_conversation_started_7d`...).
- `list_pixels`: último disparo do pixel.

## Passo 4: Veredito (um por execução, o mais grave primeiro)
- `ALERTA_CONTA`: `account_status` ≠ 1 ou pagamento pendente.
- `ALERTA_GASTO_SEM_CONVERSAO`: `spend > 0` nas 24h e nenhuma action de conversão.
- `ALERTA_PIXEL_PARADO`: pixel sem disparo recente com campanha ativa gastando.
- `ALERTA_VARIOS_CONJUNTOS_ATIVOS`: mais de um conjunto ativo na mesma campanha.
- `OK_CONVERSOES_CHEGANDO`: gasto > 0 e pelo menos 1 conversão contada.
- `OK_SEM_GASTO`: sem gasto no período (informativo; se há campanha ACTIVE e `account_status` = 1, citar).

## Passo 5: Gravar
Escrever em `outputs/ads/pixel-checks/<YYYY-MM-DD>.log` (criar a pasta se não existir):

```
=== CHECAGEM PIXEL/CONVERSAO <DATA> <HORA> ===
Veredito: <VEREDITO>
Conta: <produto> (<account_id>)  Status: <account_status>  Resta do limite: R$X
Gasto no periodo: R$X
Conversoes contadas: N (tipos: ...)
Conjuntos ativos: <nome> (<id>), ...
Ultimo disparo do pixel: <data/hora>
Actions relevantes: <lista bruta ou "nenhuma encontrada">
```

## Passo 6: Encerrar
Imprimir 1-2 linhas: `PIXEL-CHECK <DATA>: <veredito>. Gasto R$X. N conversoes.`
Em caso de alerta, prefixar com `ALERTA:` e deixar bem visível no início. Nunca corrigir sozinho.
