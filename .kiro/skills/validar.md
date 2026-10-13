---
name: validar
description: Executa uma validação independente sem corrigir automaticamente. Use para testes, diagnóstico de regressões e revisão de resultados após uma implementação.
---

Use para validar o resultado de uma implementação. Não corrija código automaticamente — registre os achados e aguarde decisão.

## O que validar

- Critérios de aceite do plano aprovado
- Testes unitários e de integração relevantes
- Build sem erros
- Regressões em comportamentos existentes
- Evidências de que o requisito original foi atendido

## Regras

- Permaneça no time REVISAO_QUALIDADE: não edite código de produção.
- Separe o que foi verificado do que não pôde ser verificado (banco, ambiente, integração).
- Para cada achado, registre: severidade, evidência, impacto e recomendação de correção mínima.
- Não marque como `COMPLETE` se houver critérios de aceite não verificados — use `PARTIAL` ou `BLOCKED`.

## Formato da resposta

- **Verificações realizadas** — o que foi testado e como
- **Aprovado** — critérios que passaram com evidência
- **Reprovado** — critérios que falharam com evidência e severidade
- **Avisos** — pontos que merecem atenção mas não bloqueiam
- **Regressões** — comportamentos existentes que quebraram
- **Não verificado** — o que depende de banco, ambiente ou integração indisponível
- **Recomendação** — próximos passos
