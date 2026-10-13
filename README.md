# AGHUse Assistant

Agentes e skills especializados para desenvolvimento no AGHUse e no SFA, nativos no [Kiro](https://kiro.dev).

## O que está incluído

**6 skills operacionais**

| Skill | Uso |
|---|---|
| `analisar` | Análise técnica somente leitura — código, banco, configuração, integrações |
| `planejar` | Plano técnico com escopo, riscos e critérios de aceite |
| `implementar` | Implementação controlada com checkpoint e aprovação |
| `validar` | Validação independente sem correção automática |
| `sfa-development` | Coordenador do SFA — delega para os 4 subagentes do projeto |
| `manutencao` | Manutenção deste repositório (skills, agentes, steering) |

**10 agentes especializados**

AGHUse: `backend`, `database`, `frontend`, `qa`, `architecture`, `aghuse_assistant`

SFA: `sfa_backend`, `sfa_database`, `sfa_frontend`, `sfa_tests`

## Início rápido

### 1. Clone o repositório

```bash
git clone https://github.com/luisbiset/aghuse-assistant.git
cd aghuse-assistant
```

### 2. Configure o MCP do Redmine

```bash
cp .kiro/settings/mcp.json.example .kiro/settings/mcp.json
```

Edite `.kiro/settings/mcp.json` e substitua `/CAMINHO/ABSOLUTO/` pelo caminho real onde você clonou este repositório. Para descobrir:

```bash
pwd
```

### 3. Configure a chave do Redmine

A chave nunca vai para arquivos. Configure como variável de ambiente na sessão atual:

```bash
read -rsp "Chave da API do Redmine: " REDMINE_API_KEY && echo && export REDMINE_API_KEY
```

Para uso permanente, adicione `REDMINE_API_KEY=sua-chave` no gerenciador de segredos do sistema ou no arquivo de inicialização do shell protegido (`~/.bashrc`, `~/.zshrc`).

### 4. Abra no Kiro

Abra a pasta `aghuse-assistant` no Kiro. As skills e agentes estarão disponíveis automaticamente.

## Como usar as skills

No chat do Kiro, carregue uma skill pelo painel de skills ou referenciando com `#nome-da-skill` e descreva o que precisa. Exemplos:

```
#analisar   Investigar o comportamento do campo competência no BPA ao gravar um procedimento ambulatorial
#analisar   54698
#planejar   Adicionar validação de CNES no formulário de internação
#implementar   Implementar o plano aprovado para validação de CNES
#validar   Revisar as mudanças do PR #312
#sfa-development   Corrigir erro de competência no relatório BPA-I
```

## Atualizar

```bash
git pull
```

Reabra o workspace no Kiro para carregar as versões novas.

## Segurança

`.kiro/settings/mcp.json` está no `.gitignore` — ele contém o caminho local e nunca deve ser commitado.

Nunca commite credenciais, tokens, dados de pacientes, dados clínicos ou de faturamento.
