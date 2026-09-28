# 🤖📈 Agente de Investimentos com Claude

**Um agente de IA que vigia a tua carteira, lembra-te dos teus próprios limites e prepara ordens — mas nunca carrega no botão sem ti.**

Este repositório reúne instruções e modelos para montar, num **Projeto do Claude** (claude.ai), um agente de investimento com papel de **sentinela**. Nasceu de uso real com uma conta eToro ligada ao Claude através do novo conector **MCP** da eToro, e de uma conta Trade Republic gerida por um agregado familiar.

> ⚠️ **Isto não é aconselhamento financeiro.** O agente não é um consultor licenciado. Todas as decisões, e a responsabilidade por elas, são de quem investe. Investir envolve risco de perda de capital.

---

## Porque é que isto é interessante agora

Em 2026 a eToro abriu a conta a agentes de IA através de um servidor MCP. Com ele, o Claude consegue:

- **ler a carteira real**: posições, saldos, P&L e histórico;
- **consultar mercados e instrumentos**;
- **preparar ordens** e só as **executar depois da tua aprovação explícita**.

Junta-se a isto um conjunto de regras escritas à partida (tetos de concentração, cripto, alavancagem). O resultado é um "copiloto" com disciplina, que não se deixa levar pelo entusiasmo nem pelo pânico.

## O que está aqui

| Ficheiro | Para quê |
|---|---|
| 📘 [`guia-integracao-etoro.md`](guia-integracao-etoro.md) | **Começa aqui.** Como ligar o Claude à eToro, segurança, armadilhas e Agent Portfolios. |
| 🧭 [`instrucoes/agente-etoro.md`](instrucoes/agente-etoro.md) | Instruções do Projeto para o agente eToro: sentinela, disciplina e portão de confirmação de ordens. |
| 🏦 [`instrucoes/agente-trade-republic.md`](instrucoes/agente-trade-republic.md) | Variante para uma conta **sem API** (ex.: Trade Republic). Trabalha sobre um ledger e prepara instruções para executares na app. |
| 🗂️ [`templates/plano-carteira.md`](templates/plano-carteira.md) | Modelo de plano de carteira: alocação por baldes, execução, regras para ações individuais, fiscalidade PT. |
| 📊 [`templates/ledger.csv`](templates/ledger.csv) | Modelo de ledger de posições, a "fonte de verdade" do agente. |

## Começar em 5 minutos

1. **Liga a eToro ao Claude.** Em claude.ai → *Settings → Connectors → Add custom connector*, usa o URL `https://mcp.public-api.etoro.com` (detalhes no [guia](guia-integracao-etoro.md)). **Começa com a conta demo.**
2. **Cria um Projeto** no Claude e cola `instrucoes/agente-etoro.md` nas instruções do projeto.
3. **Ajusta o teu perfil e os teus limites** nas secções marcadas com `<!-- AJUSTAR -->`: teto por ativo, teto de cripto, alavancagem.
4. (Opcional) Preenche `templates/plano-carteira.md` e junta-o aos documentos do projeto.
5. Abre uma conversa no projeto e escreve: **"Faz o ponto de situação da carteira."**

## Como o agente se comporta

- 🛡️ **Sentinela.** Alerta-te sem pedires sobre quedas relevantes, concentração a subir, alavancagem e notícias de risco.
- ✋ **Portão de confirmação.** Prepara a ordem, mostra-te o bloco completo (instrumento, montante, stop-loss…) e só executa com o teu "sim". **Uma ordem, uma aprovação.**
- 📏 **Limites escritos à partida.** Lembra-te deles *antes* de os ultrapassares.
- 🧊 **Sem custo afundado.** Decide manter ou fechar pela tese atual, nunca pelo preço a que compraste.
- 🎲 **Sem promessas.** Trabalha com cenários e probabilidades, nunca com garantias.

> A lição que deu origem a isto: uma conta concentrada a 100% num só token que caiu ~85%. A conclusão não foi "evitar cripto", foi **evitar concentração**. Este agente existe para não repetir esse erro.

## 🔒 Privacidade e segurança

- **Nunca** faças commit de extratos, comprovativos, IBANs, NIFs ou exports da corretora. O `.gitignore` já bloqueia PDFs, extratos e a pasta `privado/`.
- **Nunca** coles passwords, códigos 2FA ou chaves de API no chat. A ligação à eToro faz-se sempre pela janela oficial de login.
- Nas permissões do conector, deixa `place-trade` e `place-close` em **"pedir sempre aprovação"**.

## Contribuir

Ideias, correções e variantes para outras corretoras são bem-vindas. Abre uma *issue* ou um *pull request*.

---

*Feito com o Claude. Usa, adapta e partilha, mas investe com a tua cabeça.*
