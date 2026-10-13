---
name: manutencao
description: Mantém exclusivamente o AGHUse Assistant — seus skills, steering files, agentes, hooks e documentação neste repositório. Não use para alterar o AGHUse clínico/faturamento.
---

Use para analisar, planejar, implementar ou validar mudanças no próprio AGHUse Assistant. O agente responsável é `aghuse_assistant`.

## Escopo permitido

- Skills em `.kiro/skills/`
- Steering files em `.kiro/steering/`
- Agentes em `.kiro/agents/`
- Hooks em `.kiro/hooks/`
- Configurações em `.kiro/settings/`
- Documentação em `docs/` e `README.md`

## Fora do escopo

- Projeto AGHUse clínico/faturamento
- Projeto SFA
- Banco de dados compartilhado
- WildFly, Redmine ou qualquer ambiente externo

## Regras

- Preserve alterações existentes; exija checkpoint antes de mudanças mutáveis.
- Ao alterar uma skill, verifique se o frontmatter está correto e se a instrução permanece consistente com os contratos.
- Ao alterar um agente, verifique se os limites de escopo estão preservados.
- Nunca persista credenciais, URLs privadas, dados clínicos ou prompts.
- Use `UNKNOWN/NOT_CONFIRMED` quando a evidência não existir.

## Formato da entrega

- **Escopo** — o que foi alterado e por quê
- **Arquivos alterados** — lista com descrição da mudança
- **Testes/validações executados** — o que foi verificado
- **Pendências** — o que ficou fora do escopo ou bloqueado
- **Riscos** — impactos potenciais da mudança
- **Rollback** — como reverter se necessário
