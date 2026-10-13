---
inclusion: auto
---

# Política global do AGHUse Assistant

As skills operacionais (`analisar`, `planejar`, `implementar`, `validar`, `manutencao`) devem ser ativadas explicitamente pelo usuário. Tarefas comuns, consultas e explicações não iniciam o runtime operacional automaticamente.

## Classificação obrigatória antes do trabalho

Antes de qualquer trabalho substantivo, classifique a tarefa:

| Complexidade | Agentes | Paralelismo | Fluxo típico |
|---|---:|---:|---|
| `TRIVIAL` | até 1 | 1 | fast path ou um especialista |
| `LOCALIZED` | até 2 | 1 | especialista; validação direcionada |
| `TRANSVERSAL` | até 4 | 2 | discovery; especialistas necessários; fechamento |
| `CRITICAL` | até 6 | 2 | fluxo transversal; reviewer; gate reforçado |

`risk_class` é independente da complexidade: `LOW` não exige reviewer; `MEDIUM` exige QA ou auditor; `HIGH` exige QA, auditor e reviewer.

## Times de agentes

- **ANALISE**: DISCOVERY e PLAN_READY em modo somente leitura.
- **DESENVOLVIMENTO**: único time que altera código. Atua em IMPLEMENTING.
- **REVISAO_QUALIDADE**: VALIDATING, REVIEW_READY e HOMOLOGATION_READY com conclusão independente.

O fluxo entre times é sequencial. Paralelismo ocorre somente dentro do mesmo time entre frentes independentes.

## Regras gerais

- Escolha o menor número de agentes suficiente para a tarefa.
- Todo retry exige progresso verificável ou mudança concreta de estratégia.
- Nunca persista credenciais, URLs privadas, dados clínicos, de faturamento ou prompts.
- Finding crítico retorna a `IMPLEMENTING`; QA não corrige o código avaliado.
- Para mudanças relevantes, gere um Technical Handoff proporcional com evidências reais.
- Use `UNKNOWN/NOT_CONFIRMED` quando a evidência não existir — nunca invente valores.

## Segurança (SEC-001)

Nunca registre, exponha nem persista: credenciais, tokens, senhas, URLs privadas, dados de pacientes, dados clínicos, dados de faturamento, identificadores pessoais ou conteúdo de arquivos BPA. Redigir valores sensíveis em diagnósticos. Não conecte a Oracle, PostgreSQL, LDAP, WildFly, Redmine ou serviços reais sem autorização explícita.
