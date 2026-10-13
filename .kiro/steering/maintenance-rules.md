---
inclusion: auto
---

# Regras de manutenção deste repositório

Este repositório é a fonte central dos agentes e skills do AGHUse Assistant para o Kiro.

## Regras obrigatórias

- Nunca grave credenciais, tokens, URLs privadas, dados clínicos ou de faturamento em nenhum arquivo deste repositório.
- `.kiro/settings/mcp.json` está no `.gitignore` e nunca deve ser commitado — contém caminhos locais.
- Não crie commit, tag, remote ou push sem solicitação explícita do proprietário.

## Ao alterar uma skill

- Verifique se o frontmatter YAML está presente com `name` e `description`.
- Verifique se não há referências a sistemas externos de execução (Codex, OpenAI API direta, provider HTTP local).
- Verifique se a instrução é clara e autocontida — a skill não deve depender de arquivos externos ao repositório.

## Ao alterar um agente

- Verifique se o frontmatter tem `name` e `description`.
- Verifique se os limites de escopo estão explícitos (o que o agente pode e não pode fazer).
- Verifique se não há credenciais, URLs privadas ou dados sensíveis no conteúdo.

## Ao alterar um steering file

- Verifique se o frontmatter tem `inclusion` definido (`auto` ou `manual`).
- Steering com `inclusion: auto` é injetado em toda sessão — mantenha conciso e relevante.

## Validação

Após qualquer mudança neste repositório, revise manualmente:
1. O frontmatter de todos os arquivos alterados está correto.
2. Nenhum arquivo novo contém credenciais ou dados sensíveis.
3. O `.gitignore` continua cobrindo `mcp.json` e arquivos de ambiente.

## Não execute

- Operações reais no Redmine, banco Oracle, WildFly ou qualquer serviço compartilhado durante manutenção deste repositório.
- Scripts que dependam de ambiente externo (`scripts/validate.py`, `scripts/doctor.py` do projeto legado) — eles não se aplicam a este repositório.
