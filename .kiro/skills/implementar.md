---
name: implementar
description: Executa uma implementação controlada com checkpoint e aprovação. Use somente após um plano aprovado. Requer confirmação explícita antes de mudanças mutáveis.
---

Use para executar mudanças aprovadas no código. Não contorne gates, não inicie sem plano aprovado e não ignore checkpoints.

## Pré-condições

- Deve existir um plano aprovado (saída da skill `planejar`) antes de iniciar.
- Mudanças mutáveis (edição de arquivos, scripts de banco) exigem confirmação explícita do usuário.
- Classifique complexidade e risco antes de iniciar — eles determinam o budget de agentes e o nível de revisão necessário.

## Durante a implementação

- Preserve mudanças existentes; confira o estado do Git antes de editar.
- Faça a menor mudança coerente com os padrões locais e o plano aprovado.
- Atribua ownership de arquivo: um arquivo só pode ter um agente responsável por estágio.
- Não edite arquivos fora do escopo definido no plano.
- Registre evidências reais — não invente símbolos nem resultados de build.

## Formato da entrega

- **Mudanças realizadas** — arquivos alterados com descrição do que mudou
- **Testes executados** — resultado real de cada comando de build/teste
- **Itens pendentes** — o que ficou fora do escopo ou bloqueado
- **Notas de rollback** — como reverter se necessário
- **Aprovação requerida** — qualquer passo que ainda precise de confirmação humana

## Limites

- Não execute deploy, commit, push, Redmine, DDL/DML em banco compartilhado ou ação externa sem autorização explícita do usuário.
- Nunca persista credenciais, URLs privadas, dados clínicos ou de faturamento.
- Se encontrar requisito ambíguo, contrato conflitante ou risco não previsto no plano: pare, documente e aguarde orientação.
