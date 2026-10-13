---
inclusion: manual
---

# Protocolo operacional AGHUse Assistant

## Estados de execução

```
NEW → DISCOVERY → PLAN_READY → PLAN_APPROVED → IMPLEMENTING
    → VALIDATING → REVIEW_READY → HUMAN_GATE → HOMOLOGATION_READY → DONE
```

Falha de validação retorna a `IMPLEMENTING`. Qualquer estágio pode ir a `BLOCKED`. Tarefas triviais ou localizadas cujo pedido já autoriza e delimita a implementação podem ir de `NEW` diretamente a `PLAN_APPROVED`.

## Rastreamento de hipóteses

Use os estados abaixo para rastrear cada ponto de investigação:

- `OPEN` — pergunta formulada, não investigada
- `DISCOVERED` — referência encontrada, não confirmada
- `VERIFIED` — confirmado por evidência direta
- `CONCLUDED` — encerrado com conclusão técnica
- `BLOCKED` — impedido por limitação de contexto, banco ou ambiente

Uma referência encontrada é apenas `DISCOVERED`. Siga chamadas, consultas, DTOs/VOs, validações, consumidores e saídas antes de concluir.

## Gate de suficiência

Separe o status da execução do status técnico da investigação:

- `COMPLETE` — todas as perguntas obrigatórias concluídas com fontes reais
- `PARTIAL` — investigação incompleta, perguntas pendentes registradas
- `BLOCKED` — impedida por limitação de ambiente, banco ou contexto

Não force conclusões quando a evidência não existir. Preserve perguntas pendentes e limitações explicitamente.

## Adaptive Reasoning

Score determinístico por sinais da tarefa:

| Sinal | Peso |
|---|---:|
| Impacto arquitetural | 3 |
| Crítico em produção | 3 |
| Sensível a segurança | 3 |
| Migração de banco | 2 |
| Múltiplos módulos (≥2) | 2 |
| Muitos arquivos (≥6) | 2 |
| Testes obrigatórios | 1 |

- Score 0–3: `INSTANT` (tarefas simples, fast path)
- Score 4–8: `MEDIUM` (trabalho normal)
- Score ≥9: `HIGH` (tarefas críticas)

Somente `MEDIUM` pode escalar para `HIGH` uma vez, após falha verificável de teste. `HIGH` nunca escala novamente.

## Limites de budget

| Complexidade | Chamadas de modelo | Agentes |
|---|---:|---:|
| TRIVIAL | 1 | 1 |
| LOCALIZED | 3 | 2 |
| TRANSVERSAL | 5 | 4 |
| CRITICAL | 6 | 6 |

Budget global: máximo 3 tentativas, 3 retries, 3 filhos, profundidade 3, teto absoluto de 8 chamadas.

## Handoff e evidência

- Requisitos conhecidos recebem IDs; cada arquivo aponta para requisito, finalidade e owner.
- Evidência observada permanece separada da interpretação.
- Validações não executadas são registradas, não omitidas.
- Um arquivo compartilhado possui um único owner por estágio.

## Stop conditions

Pare antes de escrever quando houver:
- Requisito funcionalmente ambíguo
- Contrato compartilhado fora do escopo
- Teste contraditório
- Validação não reproduzível
- Contexto insuficiente

Use `NEEDS_EXPLANATION` quando uma solução aparentemente correta não puder ser explicada por causa, ponto da correção, comportamento preservado, evidência e rollback.

## Times de agentes (FLOW-005)

O runtime seleciona o time pelo estado atual e o menor conjunto de especialistas necessário.

### ANALISE — somente leitura

**Estados**: `DISCOVERY`, `PLAN_READY`
**Pode escrever**: não
**Agentes**: `architecture`, `database`

### DESENVOLVIMENTO — pode alterar código

**Estados**: `IMPLEMENTING`
**Pode escrever**: sim
**Agentes**: `backend`, `frontend`, `database`, `qa`, `aghuse_assistant`

### REVISAO_QUALIDADE — validação independente

**Estados**: `VALIDATING`, `REVIEW_READY`, `HOMOLOGATION_READY`
**Pode escrever**: não
**Agentes**: `qa`, `architecture`

### Regras de seleção

- Selecione sempre o **menor** número de agentes suficiente. Nunca selecione todos por padrão.
- O implementador não pode ser o revisor final.
- Coordenadores LLM não contam como chamadas de modelo — use zero chamadas de coordenação.
- Agentes do time ANALISE podem atuar com `team=ANALISE` sem escrita, mesmo que também pertençam ao DESENVOLVIMENTO.

### Distribuição de budget por complexidade (soft targets)

| Time | TRIVIAL | LOCALIZED | TRANSVERSAL | CRITICAL |
|---|---|---|---|---|
| ANALISE | 0–15% | 10–25% | 20–30% | 25–35% |
| DESENVOLVIMENTO | 70–100% | 50–70% | 40–60% | 35–50% |
| REVISAO_QUALIDADE | 0–30% | 20–35% | 25–40% | 30–40% |

---

## Responsabilidade humana

Gate anterior nunca autoriza Redmine, banco, deploy, commit ou push. Cada ação externa exige pedido próprio.
