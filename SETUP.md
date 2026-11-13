# Instalação em um computador novo

Guia para deixar as skills de ads funcionando do zero. Tempo estimado: 30 a 45 minutos.
Para quem não é técnico: siga na ordem e não pule o passo 9 (teste).

## O que você vai precisar ter em mãos
- [ ] Acesso de **administrador** ao Business Manager do CCBEU (https://business.facebook.com/settings)
- [ ] A conta de anúncios do CCBEU dentro desse Business Manager, com forma de pagamento ativa
- [ ] Acesso ao GitHub do repo `felipe-ccbeu/meta-ads-mcp` (peça ao Felipe para te convidar) **ou** a pasta dele copiada
- [ ] Um computador com Windows, macOS ou Linux e internet

---

## Parte A: Programas

### 1. Node.js (versão 20.6 ou maior)
1. Baixe a versão **LTS** em https://nodejs.org e instale com as opções padrão.
2. Abra o terminal (Windows: PowerShell) e rode:
   ```
   node --version
   ```
   Deve aparecer `v20.6` ou maior (ex.: `v22.x`). Se for menor, reinstale. O `.env` só é lido pelo MCP
   a partir da 20.6.

### 2. Git
Baixe em https://git-scm.com/downloads e instale com as opções padrão. Teste: `git --version`.

### 3. Claude Code
Siga a instalação oficial em https://claude.ai/code (ou https://docs.claude.com/en/docs/claude-code/overview).
Faça login com a conta de vocês. Teste: `claude --version`.

---

## Parte B: Baixar os projetos

### 4. Baixar o repo de automações
Escolha uma pasta (exemplo `C:\projetos`) e rode:
```
cd C:\projetos
git clone https://github.com/felipe-ccbeu/ccbeu-ads-claude.git
```
Se o endereço do repo for outro, use o que o Felipe passar. Anote o caminho final, por exemplo
`C:\projetos\ccbeu-ads-claude`.

### 5. Baixar e preparar o servidor do Meta (MCP)
O MCP é o "tradutor" entre o Claude e o Meta Ads. Está em outro repo:
```
cd C:\projetos
git clone https://github.com/felipe-ccbeu/meta-ads-mcp.git
cd meta-ads-mcp
npm install
npm run build
```
Ao terminar, deve existir o arquivo `C:\projetos\meta-ads-mcp\dist\index.js`. Se não existir, o `npm run build`
falhou: copie a mensagem de erro e mande para o Felipe.

Sem acesso ao GitHub? Peça a pasta `meta-ads-mcp` inteira por pendrive ou nuvem e rode só
`npm install` e `npm run build` dentro dela.

---

## Parte C: Credenciais do Meta

Você vai coletar **3 valores obrigatórios** (token, conta de anúncios e Página) e 1 opcional (pixel).
Guarde-os num bloco de notas por enquanto e apague depois do passo 7.

### 6. Pegar os IDs

**ID da conta de anúncios**
1. Abra https://business.facebook.com/settings
2. Menu da esquerda: **Contas → Contas de anúncios**.
3. Clique na conta do CCBEU. O número que aparece (só dígitos) é o ID.
4. Em uso, ele leva o prefixo `act_`. Exemplo: `act_1234567890`.

**ID da Página do Facebook**
1. Mesma tela, **Contas → Páginas**, clique na Página que publica os anúncios.
2. Copie o número do ID.

**ID do pixel (opcional, mas recomendado)**
1. **Fontes de dados → Pixels**, clique no pixel do CCBEU e copie o ID.
2. Se não houver pixel, anote "sem pixel". As skills avisam que a medição de conversão não existe.

### 7. Criar o token de acesso (a parte mais delicada)

O token é a "senha" que dá acesso à API. Crie um **Usuário do sistema**, que gera um token que não expira.

**7.1 Criar um app (uma vez só)**
1. Abra https://developers.facebook.com/apps e clique em **Criar app**.
2. Caso de uso: **Outro**. Tipo: **Empresa** (Business).
3. Nome: por exemplo "CCBEU Ads". Vincule ao Business Manager do CCBEU.
4. Dentro do app, adicione o produto **API de Marketing**.

**7.2 Criar o usuário do sistema**
1. Em https://business.facebook.com/settings: **Usuários → Usuários do sistema → Adicionar**.
2. Nome: `ads-claude`. Função: **Admin**.
3. Clique em **Atribuir ativos**:
   - **Contas de anúncios**: selecione a conta do CCBEU e dê controle total (**Gerenciar campanhas**)
     se for criar/editar anúncios. Para **só leitura**, basta **Ver desempenho**.
   - **Páginas**: selecione a Página do CCBEU (necessário para criar anúncios).
4. Se o app ainda não aparecer, vá em **Contas → Apps → Adicionar** e adicione o app do passo 7.1.

**7.3 Gerar o token**
1. Selecione o usuário `ads-claude` e clique em **Gerar token**.
2. Escolha o app "CCBEU Ads".
3. Validade: **Nunca expira** (se a opção existir).
4. Marque as permissões:
   - `ads_read`: obrigatória.
   - `ads_management`: **só** se for criar ou alterar anúncios.
   - `business_management`: opcional.
5. Clique em **Gerar token** e **copie na hora**. O Meta não mostra de novo. Se perder, gere outro.

> **Segurança:** o token dá acesso à conta de anúncios, e portanto ao dinheiro. Não mande por WhatsApp, e-mail
> nem cole no chat do Claude. Escreva só no arquivo `.env` do passo 8.

---

## Parte D: Configurar o repo

### 8. Preencher o `.env`
1. Abra a pasta `ccbeu-ads-claude`.
2. Copie `.env.example` e renomeie a cópia para `.env` (se o `.env` já existir, use ele).
3. Abra o `.env` num editor (Bloco de Notas serve) e preencha **sem aspas e sem espaços**:

| Linha | O que colocar |
|---|---|
| `META_ACCESS_TOKEN=` | O token do passo 7.3 |
| `META_AD_ACCOUNT_ID=` | `act_` + o número da conta (passo 6) |
| `META_PAGE_ID=` | O ID da Página (passo 6) |
| `PRODUCT_LABEL=` | `CCBEU` |
| `DEFAULT_LINK_URL=` | O site para onde os anúncios apontam (ex.: `https://...`) |
| `DEFAULT_COUNTRIES=` | `BR` (ou outros países, separados por vírgula) |
| `MAX_DAILY_BUDGET=` | Teto de orçamento diário em **centavos**: `5000` = R$ 50,00. Trava de segurança. |
| `META_PIXEL_ID=` | O ID do pixel (opcional) |

Exemplo de linha correta: `META_AD_ACCOUNT_ID=act_1234567890`

O arquivo `.env` não vai para o git, então não há risco de publicar o token por engano. Não renomeie o
`.gitignore` nem apague a linha `.env` dele.

### 9. Criar o `.mcp.json`
O `.mcp.json` não vai para o git. Copie o `.mcp.json.example` com o nome `.mcp.json` e troque os caminhos
pelos do **seu** computador:

```json
{
  "mcpServers": {
    "ccbeu-meta-ads": {
      "command": "node",
      "args": [
        "--env-file=C:\\projetos\\ccbeu-ads-claude\\.env",
        "C:\\projetos\\meta-ads-mcp\\dist\\index.js"
      ]
    }
  }
}
```

- `command`: se o `node --version` do passo 1 funcionou no terminal, use só `"node"`.
- Linha 1 dos `args`: o caminho completo do seu `.env`.
- Linha 2: o caminho completo do `dist\index.js` do passo 5.
- No Windows, cada `\` precisa ser escrito duas vezes (`\\`) dentro do JSON. No macOS/Linux use `/` normal.

### 10. Preencher o contexto do produto
Abra `.agents/ccbeu-context.md` e troque todos os `PREENCHER` por informação real:
- **O que é o CCBEU**, quem é o cliente-alvo e qual dor ele resolve
- **Conversão principal** (o que conta como resultado: lead, compra, cadastro, instalação)
- **Como o resultado é medido depois do clique** (CRM, planilha, painel)
- **Metas de custo**: limite aceitável e meta ideal por conversão, e o orçamento diário atual

Se não souber algum, deixe `PREENCHER`. As skills tratam como "desconhecido" e não inventam, mas o diagnóstico
fica menos completo.

---

## Parte E: Testar

### 11. Abrir o Claude no projeto
```
cd C:\projetos\ccbeu-ads-claude
claude
```
1. O Claude vai perguntar se pode usar o servidor `ccbeu-meta-ads` do `.mcp.json`. Aprove.
2. Confira com o comando `/mcp`: o servidor deve estar **connected**.

### 12. Testar o acesso
Digite no Claude:
```
Rode o whoami do ccbeu-meta-ads e me diga produto, conta e página.
```
Resposta esperada: `CCBEU`, o ID da conta (`act_...`) e a Página.

Depois:
```
/ccbeu-ads-pixel-check
```
Deve voltar um veredito (por exemplo `OK_SEM_GASTO` ou um alerta) e gravar um arquivo em `outputs/ads/pixel-checks/`.

### 13. Primeira auditoria
```
/ccbeu-ads-audit
```
Gera um relatório HTML em `outputs/ads/`. **Ele só lê; não altera nada na conta.**

---

## Para que serve cada skill

| Comando | O que faz | Altera a conta? |
|---|---|---|
| `/ccbeu-ads-audit` | Auditoria profunda com nota e propostas | Não |
| `/ccbeu-ads-pixel-check` | Checagem rápida de conta e conversões | Não |
| `/ccbeu-ads-onepager` | Resumo de uma página do funil | Não |
| `/ccbeu-ads-report` | Relatório semanal completo | Não |
| `/ccbeu-ad-management` | Criar/editar/pausar anúncios | **Sim**, só com seu pedido, e tudo nasce pausado |

---

## Problemas comuns

| Sintoma | Causa provável e solução |
|---|---|
| `/mcp` mostra o servidor como failed | Caminho errado no `.mcp.json` (passo 9) ou `npm run build` não rodou (passo 5). |
| `faltam META_ACCESS_TOKEN, META_PAGE_ID` | O `.env` está vazio, com nome errado ou caminho errado no `--env-file`. |
| `node: bad option: --env-file` | Node antigo. Atualize para 20.6 ou maior (passo 1). |
| `(#200) Requires ads_read permission` | O token foi gerado sem `ads_read`. Gere outro (passo 7.3). |
| `Invalid OAuth access token` | Token copiado incompleto ou revogado. Gere outro. |
| Volta lista vazia ou "sem permissão" | A conta de anúncios não foi atribuída ao usuário do sistema (passo 7.2). |
| `account_status` diferente de 1 | Problema de pagamento ou conta restrita. Veja em https://business.facebook.com/billing_hub |
| Erro "Unsupported get request" | ID da conta sem o prefixo `act_` ou com número errado. |

## Segurança (resumo)
- O token vive **só** no `.env`. Nunca em chat, e-mail, print ou dentro das skills.
- Se o token vazar: em **Usuários do sistema**, revogue o token e gere outro.
- Para a v1, use permissão **só leitura** (`ads_read` + "Ver desempenho"). Libere escrita só quando for criar anúncios.
- O `MAX_DAILY_BUDGET` e o `confirm: true` do MCP impedem gasto acima do teto e ativação sem confirmação.

## Links úteis
- Configurações do negócio: https://business.facebook.com/settings
- Seus apps: https://developers.facebook.com/apps
- API de Marketing: https://developers.facebook.com/docs/marketing-apis
- Verificar um token: https://developers.facebook.com/tools/debug/accesstoken
- Cobrança: https://business.facebook.com/billing_hub
