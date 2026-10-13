---
name: redmine-workflows
description: Consulta e gerencia projetos, chamados, comentários, status, responsáveis e horas no Redmine da SESAB. Use quando precisar localizar, resumir, criar ou atualizar chamados, acompanhar pendências, registrar trabalho ou preparar triagens e relatórios.
---

# Fluxos do Redmine SESAB

Nenhuma leitura, análise, plano ou gate anterior autoriza implicitamente comentário, status, horas ou qualquer outra escrita no Redmine. Toda mutação exige pedido e confirmação próprios.

## Usar sempre a API

- Use a API REST oficial do Redmine como canal obrigatório para toda consulta, alteração, registro de horas e anexo.
- Prefira as ferramentas do servidor MCP `redmine` quando elas expuserem a operação necessária.
- Quando uma operação não estiver exposta pelo servidor MCP, chame diretamente o endpoint REST com `REDMINE_API_KEY`, preservando as mesmas confirmações e verificações deste fluxo.
- Não use automação de navegador, scraping, pesquisa web ou URLs públicas como substitutos da API.
- Se a API estiver indisponível ou não oferecer a operação necessária, pare e informe a limitação em vez de contornar pela interface.

## Conectar com segurança

- Use somente `https://redmine.saude.ba.gov.br/`, salvo alteração explícita do proprietário.
- Exija `REDMINE_API_KEY` no ambiente do processo. Nunca peça que a chave seja colada na conversa, nunca a exiba e nunca a grave em arquivos.
- Se a autenticação falhar, informe apenas o código HTTP e oriente a conferir se a API REST está habilitada e se a chave está disponível no ambiente.
- Não desabilite validação TLS e não contorne VPN, proxy ou política de rede.

## Consultar

1. Quando o usuário disser "meus chamados", obtenha primeiro o usuário atual e filtre `assigned_to_id` como `me`.
2. Liste resultados de forma concisa: ID, assunto, projeto, status, prioridade, responsável e última atualização.
3. Busque o chamado individual antes de responder perguntas sobre descrição, histórico, campos personalizados ou próxima ação.
4. Consulte metadados e membros do projeto antes de sugerir IDs de status, tracker, prioridade, atividade ou responsável.
5. Paginação: não afirme que uma lista está completa se a API indicar itens restantes.

## Alterar

Trate `create_issue`, `add_issue_note`, `update_issue`, `log_time` e upload de anexos como escrita em produção.

1. Obtenha o estado atual do chamado e os metadados necessários.
2. Prepare um resumo do efeito: chamado, campos, texto e horas que serão enviados.
3. Peça confirmação explícita imediatamente antes de executar a operação de escrita — mesmo que a intenção anterior pareça clara.
4. Após a confirmação, execute uma única vez. Não repita automaticamente uma escrita cujo resultado seja incerto por timeout.
5. Consulte novamente o chamado quando for importante comprovar o estado final.
6. Relate exatamente o que mudou e apresente o ID e o link do chamado.

Nunca encerre chamado, troque responsável, altere prioridade, edite estimativa ou lance horas sem mostrar o valor proposto na confirmação.

## Anexar arquivos

1. Consulte o chamado e valide localmente os arquivos antes do envio.
2. Mostre os nomes, a quantidade e o formato e peça confirmação explícita antes do upload.
3. Envie cada arquivo para `/uploads.json` e vincule os tokens ao chamado em uma única atualização.
4. Não compacte arquivos quando o usuário pedir anexos individuais.
5. Não repita automaticamente uploads ou vinculações cujo resultado seja incerto.
6. Confirme os IDs e nomes dos anexos efetivamente vinculados.

## Criar chamados

- Confirme projeto, tracker, assunto e descrição antes de criar.
- Resolva IDs a partir dos metadados — não adivinhe.
- Mantenha descrições factuais e rastreáveis: comportamento observado, esperado, evidência e critérios de aceite.
- Não inclua credenciais, dados de pacientes, dumps ou informação pessoal desnecessária.

## Registrar horas

- Confirme chamado ou projeto, data, quantidade de horas, atividade e comentário.
- Nunca registre horas em nome de outro usuário.
- Não arredonde nem altere a quantidade informada sem avisar.

## Resumir e triar

- Separe fatos existentes no Redmine de inferências.
- Ao priorizar, declare os critérios usados; não modifique prioridade ou status durante uma análise.
- Não reproduza dados pessoais desnecessários. Preserve decisões, bloqueios e próximas ações.
- Forneça links no formato `https://redmine.saude.ba.gov.br/issues/<id>`.

## Limites

- Respeite as permissões retornadas pelo Redmine; um erro 403 não autoriza tentativa com outra identidade.
- Não use impersonação de usuário.
- Não execute alterações em lote sem autorização específica para cada conjunto claramente delimitado.
