---
name: sfa-development
description: Coordena análise, implementação, testes e revisão no Sistema de Faturamento AGHUse (SFA). Use para tarefas que mencionem SFA, faturamento AGHUse, BPA, CNES, CBO, SIGTAP, glosas ou um repositório com sfa/pom.xml e sfa-client/angular.json.
---

# Coordenação do desenvolvimento do SFA

## Identificar o projeto

- Confirme que o repositório contém `sfa/pom.xml` e `sfa-client/angular.json` antes de aplicar estas regras.
- Localize a raiz do Git quando a sessão começar em um subdiretório.
- Não aplique convenções do SFA a outro sistema apenas porque ele também usa Java ou Angular.

## Pipeline por times

- **Time de Análise**: discovery somente leitura. Especialistas SFA podem atuar aqui, sempre sem escrita.
- **Time de Desenvolvimento**: edição controlada de código. Atribua ownership de arquivos sem sobreposição.
- **Time de Revisão e Qualidade**: validação independente. QA não corrige o código avaliado.

Selecione o conjunto mínimo de agentes, preserve ownership e produza handoff antes de avançar ao próximo time.

## Missão antes de delegar

Classifique a tarefa antes de iniciar:

| Complexidade | Agentes | Fluxo |
|---|---:|---|
| TRIVIAL | até 2 | um especialista + auditor opcional |
| LOCALIZED | até 3 | especialistas necessários |
| TRANSVERSAL | até 6 | discovery + especialistas + fechamento |
| CRITICAL | até 8 | fluxo completo + reviewer obrigatório |

Declare também: risco, modo operacional, sinais da tarefa, budget e ownership de arquivos. Exceder o budget exige justificativa no handoff.

## Subagentes SFA

| Agente | Propriedade |
|---|---|
| `sfa_backend` | controllers, services, VOs, segurança, regras BPA/faturamento, testes Java em `sfa/` |
| `sfa_database` | entidades JPA, repositories, datasources, consultas, scripts SQL |
| `sfa_frontend` | telas, formulários, rotas, services HTTP, models em `sfa-client/` |
| `sfa_tests` | testes JUnit/Mockito e Jasmine/Karma, regressão, fixtures, cobertura |

## Delegar corretamente

1. Para tarefa restrita a um domínio, delegue ao especialista correspondente.
2. Para tarefa transversal, delegue a análise em paralelo quando as frentes forem independentes.
3. Durante implementação paralela, atribua arquivos sem sobreposição. Dois agentes nunca editam o mesmo arquivo.
4. Trate o contrato como handoff explícito: frontend descreve a necessidade de API; backend define request/response e regras; banco define persistência e fronteiras transacionais.
5. Se o contrato ainda estiver ambíguo, sequencie: banco → backend → frontend.

## Contratos de código

**Backend** (Java 11, Spring Boot 2.5.2):
- Preserve `javax.*`, Spring Boot 2.5.2 e WAR.
- Use `BigDecimal` para valores monetários.
- Em BPA: preserve posições fixas, competência, zeros à esquerda e `ISO_8859_1`.
- Mantenha Oracle como datasource principal e PostgreSQL como secundário — não misture transaction managers.
- `hibernate.hbm2ddl.auto=none` — schema administrado externamente.

**Frontend** (Angular 12, RxJS 6, TypeScript 4.3):
- Preserve NgModule, Angular Material 12 e Karma/Jasmine.
- Não introduza standalone components, signals ou APIs de Angular moderno.
- Use `environment.apiUrl`; nunca grave URLs de ambiente no código.
- Não edite `dist/`, `node_modules/` nem artefatos copiados para o WAR.

## Segurança e dados sensíveis

- Dados de pacientes, profissionais e faturamento são sensíveis — nunca exponha em respostas, logs, fixtures ou commits.
- Nunca revele credenciais, tokens, senhas, endereços internos ou strings de conexão.
- Não conecte a Oracle, PostgreSQL, LDAP ou serviços reais sem autorização explícita.

## Build e validação

```bash
cd sfa && bash ./mvnw test
bash ./mvnw package

cd ../sfa-client
npm test -- --watch=false --browsers=ChromeHeadless
npm run build -- --configuration development
```

Execute `npm ci` somente quando as dependências precisarem ser restauradas. Não aponte testes para infraestrutura real.

## Concluir

Consolide o handoff com: resultado, evidências, arquivos alterados, contrato afetado, validações executadas, riscos, limitações e próxima responsabilidade. Informe qualquer validação impedida por banco, LDAP, navegador ou ambiente.
