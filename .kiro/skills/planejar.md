---
name: planejar
description: Cria um plano técnico independente sem alterar arquivos. Use para definir escopo, estratégia, riscos, critérios de aceite e testes antes de iniciar uma implementação.
---

Use para criar um plano técnico sem alterar arquivos. Não inicie implementação automaticamente ao final.

## O que o plano deve conter

- **Objetivo** — o que precisa ser feito e por quê
- **Premissas** — o que foi assumido e o que ainda precisa ser confirmado
- **Passos** — sequência de implementação com responsável por passo (agente ou time)
- **Dependências** — entre passos, módulos e agentes
- **Critérios de aceite** — comportamento esperado verificável
- **Plano de validação** — testes, build e revisões necessárias
- **Riscos** — o que pode dar errado e como mitigar

## Regras

- Escolha o menor conjunto de agentes suficiente para a tarefa.
- Classifique a complexidade (TRIVIAL / LOCALIZED / TRANSVERSAL / CRITICAL) e o risco antes de propor o plano.
- Se o requisito estiver ambíguo, registre a lacuna e não invente comportamento esperado.
- Não inclua passos que dependam de banco, deploy, Redmine ou serviços externos sem registrar essa dependência explicitamente.
- Preserve a separação entre times: ANALISE planeja, DESENVOLVIMENTO implementa, REVISAO_QUALIDADE valida.
