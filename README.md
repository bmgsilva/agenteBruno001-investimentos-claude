# Agente de Investimentos com Claude

Instruções e modelos para montar, num **Projeto do Claude** (claude.ai), um agente de análise de carteira com papel de **sentinela**: acompanha posições, lembra limites, alerta riscos e prepara ordens — mas **nunca executa nada sem validação humana**.

> ⚠️ Isto não é aconselhamento financeiro. O agente não é consultor licenciado; as decisões e a responsabilidade são sempre de quem investe.

## O que está aqui

| Ficheiro | Para quê |
|---|---|
| `guia-integracao-etoro.md` | **Comece aqui.** Passo a passo para ligar o Claude à eToro (conector MCP), segurança, armadilhas e Agent Portfolios. |
| `instrucoes/agente-etoro.md` | Instruções de projeto para um agente ligado à eToro via conector MCP (lê carteira, prepara ordens com portão de confirmação). |
| `instrucoes/agente-trade-republic.md` | Instruções para um agente de acompanhamento de uma conta sem API (Trade Republic): trabalha sobre um ledger e prepara instruções para executar na app. |
| `templates/plano-carteira.md` | Modelo de plano de carteira: alocação por baldes, regras de execução, satélite de ações individuais, notas fiscais (PT). |
| `templates/ledger.csv` | Modelo de ledger de posições para servir de "fonte de verdade". |

## Como usar

1. Em claude.ai, cria um **Projeto** novo.
2. Copia o conteúdo de um dos ficheiros em `instrucoes/` para as **instruções do Projeto** e ajusta o perfil e os limites (secções marcadas com `<!-- AJUSTAR -->`).
3. Preenche `templates/plano-carteira.md` com os teus números e adiciona-o aos documentos do Projeto (e o ledger, se usares).
4. **eToro:** liga o conector eToro ao Claude — ver `guia-integracao-etoro.md`.
5. Começa a sessão com algo como: *"Faz o ponto de situação da carteira."*

## Princípios de desenho

- **Sentinela, não piloto automático:** alertas proativos (quedas, concentração, alavancagem, limites).
- **Portão de confirmação:** uma ordem por aprovação explícita; nada agrupado sob um único "ok".
- **Limites escritos à partida:** teto por ativo, teto de cripto, alavancagem — o agente lembra-os antes de serem ultrapassados.
- **Custo afundado:** decisões pela tese atual, nunca pelo preço de compra.
- **Sem promessas:** cenários e probabilidades, nunca garantias.

## Privacidade

Não faças commit de extratos, comprovativos, IBANs, NIFs ou exports da corretora. O `.gitignore` já exclui PDFs, CSVs de extratos e a pasta `privado/`.
