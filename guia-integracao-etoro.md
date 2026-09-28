# Guia — Integrar o Claude com a eToro

A eToro abriu a conta a agentes de IA através de um servidor **MCP** (Model Context Protocol). Na prática, o Claude passa a conseguir **ler a tua carteira real**, consultar mercados e, com a tua confirmação, **preparar e colocar ordens**. Este guia explica como ligar tudo e como o fazer com segurança.

> ⚠️ Com isto, o Claude pode mexer em dinheiro real. Lê a secção de segurança **antes** de ligares uma conta real. Informação verificada em setembro de 2026; a funcionalidade está a evoluir depressa, por isso confirma os passos na documentação oficial (links no fim).

---

## 1. Dois caminhos possíveis

| | **A. Conector MCP no claude.ai** (recomendado para começar) | **B. Agent Portfolios + API key** (avançado) |
|---|---|---|
| Para quem | Qualquer pessoa com conta eToro e Claude | Quem quer um agente autónomo (Claude Code, scripts) |
| Autenticação | Login eToro no browser (OAuth + 2FA) | Chave de API com âmbito limitado |
| O que o agente vê | A tua conta | Só um sub-portfólio dedicado |
| Quem executa | Tu aprovas cada ordem | O agente pode operar sozinho dentro dos limites |
| Mínimo | — | $200 no sub-portfólio |

Este repositório usa o **caminho A**: o agente analisa e prepara, tu decides.

---

## 2. Caminho A — Conector MCP no claude.ai

### Pré-requisitos
- Conta eToro ativa (demo ou real) com 2FA ligado.
- Conta Claude num plano que permita conectores personalizados.

### Passos
1. Em **claude.ai → Settings → Connectors**, escolhe **Add custom connector**.
2. Preenche:
   - **Nome:** `etoro`
   - **URL:** `https://mcp.public-api.etoro.com`
3. Guarda e carrega em **Connect**. Abre-se uma janela de login da eToro. Entra, confirma o 2FA e aprova o acesso.
4. Cria um **Projeto** novo no Claude e cola as instruções de `instrucoes/agente-etoro.md` nas instruções do projeto, ajustando o teu perfil e os limites.
5. Numa conversa dentro do projeto, confirma que o conector está ativo (menu de ferramentas da conversa) e escreve:
   *"Faz o ponto de situação da carteira."*

### Ferramentas que o agente passa a ter (principais)
| Ferramenta | Faz o quê |
|---|---|
| `get-my-portfolio-summary` | Valor, posições, P&L |
| `get-my-balances` | Saldos (útil para perceber onde está o dinheiro) |
| `get-my-positions-and-orders` | Posições abertas e ordens pendentes |
| `get-my-trading-history` | Histórico de operações |
| `get-instruments-overview` | Dados de instrumentos e mercado |
| `get-my-watchlists` | As tuas watchlists |
| `prepare-trade` / `prepare-close` | **Prepara** uma ordem e devolve um bloco de confirmação (não executa) |
| `place-trade` / `place-close` | **Executa** a ordem preparada |

O desenho em dois passos (*prepare* → *place*) é o que permite ter um **portão de confirmação**: o agente mostra-te exatamente o que vai fazer e só executa depois do teu "sim".

### Configuração de segurança recomendada
- Nas permissões do conector no Claude, põe `place-trade` e `place-close` em **"Pedir sempre aprovação"**, nunca em "permitir sempre". Assim ficas com dois travões: a regra nas instruções e a aprovação da própria app.
- **Começa numa conta demo.** Faz algumas sessões e algumas ordens de teste antes de ligares a conta real.

---

## 3. Coisas que aprendemos a usar isto (armadilhas)

- **Depósitos não aparecem logo para investir.** Uma transferência bancária cai primeiro na carteira de dinheiro (eToro Money/Cash), não na conta de trading. Tens de passar o dinheiro manualmente para a conta de trading antes de o `prepare-trade` o ver. Se os números não baterem, pede ao agente para usar `get-my-balances`.
- **O token de confirmação expira depressa** (cerca de 1–2 minutos). Se demorares a decidir, o agente tem de voltar a preparar a ordem. É normal.
- **Ações e ETFs americanos exigem o formulário W-8BEN** preenchido na conta eToro. Sem ele, ordens como SPY ou NVDA são rejeitadas.
- **Fora do horário de mercado**, ordens de ações dos EUA ficam em espera (`WaitingForMarket`) e executam na abertura. O dinheiro fica reservado logo. Cripto executa 24/7.
- **Algumas ferramentas pedem aprovação e podem expirar em silêncio** (ex.: `get-instruments-overview`). Se nada acontecer, tenta de novo uma vez ou verifica o conector.
- **A taxa overnight** mostrada ao preparar uma ordem é por unidade inteira. Em posições pequenas, fracionadas e sem alavancagem, é desprezável.

---

## 4. Caminho B — Agent Portfolios (para quem quer ir mais longe)

A eToro lançou em março de 2026 os **Agent Portfolios**: sub-portfólios separados, financiados à parte, onde um agente opera com uma **chave de API de âmbito limitado** e só tem acesso a esse sub-portfólio. Servem agentes que executam código (Claude Code, Cursor, etc.), não chatbots.

- Mínimo de $200 por sub-portfólio.
- Isola o dinheiro do agente do resto dos teus investimentos. Se correr mal, o estrago fica lá dentro.
- Não disponível para clientes dos EUA.
- Para Claude Code, a eToro disponibiliza uma skill em `https://mcp.public-api.etoro.com/skill`, que regista o servidor MCP e trata da autenticação. As chaves e documentação estão no portal de programadores da eToro.

**Aviso:** um agente autónomo pode executar ordens muito depressa. Só vale a pena com regras de risco escritas em código (tamanho máximo por ordem, stop-loss obrigatório, limite diário de perdas) e depois de testar em demo.

---

## 5. Regras de ouro

1. **Nunca partilhes a password, o 2FA ou chaves de API no chat.** A ligação faz-se sempre pela janela oficial de login.
2. **Uma ordem, uma aprovação.** Nada de "ok, faz tudo".
3. **Limites escritos antes de começar:** teto por ativo, teto de cripto, alavancagem. O agente existe para te lembrar deles.
4. **O agente não é consultor financeiro.** Usa-o para pensar melhor e ter disciplina, não para delegar a responsabilidade.
5. **Podes cortar o acesso a qualquer momento:** desliga o conector no Claude ou revoga a aplicação na tua conta eToro.

---

## Links oficiais
- Guia Claude Code, eToro Builders: https://builders.etoro.com/tools/claude-code
- Ajuda eToro, "How do I connect AI to my eToro account?": https://help.etoro.com/s/article/How-do-I-connect-AI-to-my-etoro-account
- Anúncio dos Agent Portfolios: https://www.etoro.com/news-and-analysis/etoro-updates/agent-portfolios-let-your-ai-agent-trade-for-you/
