---
name: analisar
description: Executa uma análise técnica independente, em modo somente leitura, sobre projeto, tarefa ou escopo. Use para investigar código, banco, configuração e integrações — com ou sem referência a chamado Redmine.
---

Use para executar uma análise técnica independente, em modo somente leitura, sobre um projeto, tarefa ou escopo. Não inicie outra operação ao final.

O Redmine é apenas uma fonte de contexto e requisitos. A entrega é a análise técnica realizada sobre o código, configuração, banco e integrações do projeto. Nunca encerre a operação apenas com um resumo do chamado.

A resposta deve conter uma matriz de impacto por componente, com papel identificado, alteração, classificação, justificativa e evidência. Não apresente apenas uma lista de arquivos.

## Fluxo obrigatório

Antes das buscas, formule perguntas técnicas verificáveis para cada requisito explícito. Use os estados `OPEN`, `DISCOVERED`, `VERIFIED`, `CONCLUDED` e `BLOCKED` para hipóteses; evidências placeholder não confirmam requisitos.

Uma referência encontrada é apenas `DISCOVERED`: siga chamadas, consultas, DTOs/VOs, validações, consumidores e saídas relevantes antes de concluir.

### Quando houver número de tarefa

1. Consulte o chamado e, quando houver referências, consulte também os chamados relacionados necessários para descobrir o requisito completo, o comportamento esperado, limites, regras de aceite e evidências.
2. Extraia do Redmine fatos e requisitos, separando-os de inferências.
3. Localize no projeto os módulos, telas, entidades, tabelas, DAOs, serviços, validações, relatórios, integrações, auditoria e testes relacionados ao requisito.
4. Rastreie o dado ou comportamento ponta a ponta: entrada, transporte, regra, persistência, leitura, saídas e histórico. Para cada ponto encontrado, classifique se precisa ser alterado, apenas validado ou está fora do escopo.
5. Confronte o requisito com a implementação atual e produza conclusão técnica: causa/estado atual, impacto, arquivos e banco afetados, riscos, lacunas de evidência, recomendação e testes necessários.

Se o requisito ou o novo comportamento esperado não estiver definido, registre a lacuna explicitamente — não invente valores.

## Gate de suficiência

Separe o status da execução do status técnico da investigação. Use `COMPLETE` somente quando todas as perguntas obrigatórias estiverem concluídas com fontes reais; caso contrário, use `PARTIAL` ou `BLOCKED` e preserve as perguntas pendentes e limitações.

## Formato da resposta

Apresente em seções Markdown:

- **Resumo** — contexto e escopo investigado
- **Causa / Estado atual** — quando aplicável
- **Arquivos envolvidos** — com papel de cada um
- **Banco** — tabelas, consultas e impactos identificados
- **Riscos** — classificados por severidade
- **Recomendação** — próximos passos técnicos
- **Não confirmado** — pontos que dependem de banco, ambiente ou integração indisponível

Diferencie claramente:
- **Fato do Redmine**: requisito, decisão, comentário ou chamado relacionado.
- **Evidência do código**: arquivo, símbolo, linha ou mapeamento observado.
- **Conclusão técnica**: impacto ou alteração deduzida das evidências.
- **Não confirmado**: ponto que depende de banco, ambiente, requisito ou integração não disponível.
