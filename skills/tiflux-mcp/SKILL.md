---
name: tiflux-mcp
description: Skill da REST API do Tiflux na MCP.AI: 30 endpoints em /api/tiflux. Wrapper da API oficial v2 do Tiflux (help desk e service desk): chamados com respostas ao solicitante, notas internas, anexos, histórico de estágios e SLAs, apontamentos de horas, clientes e solicitantes, mesas com estágios, prioridades e catálogo de serviços, base de conhecimento, contratos e relatórios de faturamento e satisfação. Consulta e operação. Autenticação por token de Sessão API do usuário. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Tiflux — REST API skill

Você tem acesso à **Tiflux** REST API na MCP.AI.

> Wrapper da API oficial v2 do Tiflux (help desk e service desk): chamados com respostas ao solicitante, notas internas, anexos, histórico de estágios e SLAs, apontamentos de horas, clientes e solicitantes, mesas com estágios, prioridades e catálogo de serviços, base de conhecimento, contratos e relatórios de faturamento e satisfação. Consulta e operação. Autenticação por token de Sessão API do usuário.

## Base URL

```
https://api.mcp.ai/api/tiflux
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/tiflux/billings/history \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/tiflux/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (30)

#### `tiflux_billings_history`

Histórico de faturamentos. Os pares de data são obrigatórios em conjunto: billing_start_date com billing_end_date, e due_start_date com due_end_date. _(POST /api/tiflux/billings/history)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `billing_start_date` | string | Não | Início do período de faturamento (YYYY-MM-DD). |
| `billing_end_date` | string | Não | Fim do período de faturamento (YYYY-MM-DD). |
| `due_start_date` | string | Não | Início do período de vencimento (YYYY-MM-DD). |
| `due_end_date` | string | Não | Fim do período de vencimento (YYYY-MM-DD). |
| `client_id` | string | Não | Filtra por cliente. |
| `nfe_number` | string | Não | Filtra por número da nota fiscal. |
| `ticket_number` | string | Não | Filtra por chamado. |
| `type` | string | Não | Situação do faturamento: faturado, estornado ou pago. (billed, reversed, paid) |
| `client_ids` | string[] | Não | Bulk mode: multiple values for client_id |

#### `tiflux_cancel_ticket`

Cancela um chamado, encerrando-o SEM tratá-lo como atendido (duplicado, aberto por engano, fora de escopo). _(POST /api/tiflux/cancel/ticket)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |

#### `tiflux_close_ticket`

Encerra um chamado, marcando-o como resolvido. _(POST /api/tiflux/close/ticket)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |

#### `tiflux_create_appointment`

Lança um apontamento de horas num chamado, em nome do usuário dono do token. _(POST /api/tiflux/create/appointment)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |
| `date` | string | Sim | Data do apontamento (YYYY-MM-DD). |
| `init_time` | string | Sim | Hora de início (HH:MM). |
| `end_time` | string | Sim | Hora de término (HH:MM). |
| `description` | string | Sim | Descrição do trabalho realizado. |

#### `tiflux_create_internal_communication`

Cria uma comunicação interna num chamado. _(POST /api/tiflux/create/internal/communication)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |
| `text` | string | Sim | Texto da nota interna. |
| `files` | object[] | Não | Anexos (máximo 10 arquivos, 25MB cada). |

#### `tiflux_create_ticket`

Abre um novo chamado. Resolva desk_id e client_id antes com tiflux_list_desks e tiflux_list_clients. Identifique o solicitante por requestor_id, ou pelos campos requestor_name e requestor_email quando _(POST /api/tiflux/create/ticket)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `title` | string | Sim | Título do chamado. |
| `description` | string | Sim | Descrição do problema ou solicitação. |
| `desk_id` | string | Não | Mesa de atendimento (tiflux_list_desks). |
| `client_id` | string | Não | Cliente (tiflux_list_clients). |
| `priority_id` | string | Não | Prioridade (tiflux_list_desk_priorities). |
| `status_id` | string | Não | Status inicial. |
| `services_catalogs_item_id` | string | Não | Item do catálogo de serviços (tiflux_list_desk_services_catalogs). |
| `requestor_id` | string | Não | Solicitante já cadastrado (tiflux_list_requestors). |
| `requestor_name` | string | Não | Nome do solicitante, quando não houver requestor_id. |
| `requestor_email` | string | Não | E-mail do solicitante, quando não houver requestor_id. |
| `requestor_telephone` | string | Não | Telefone do solicitante. |
| `responsible_id` | string | Não | Atendente responsável (tiflux_list_technical_users). |
| `followers` | string | Não | Seguidores do chamado, conforme a API oficial. |
| `parent_ticket_number` | string | Não | Número do chamado pai, para vincular este como referência. |
| `files` | object[] | Não | Anexos (máximo 10 arquivos, 25MB cada). |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |
| `client_ids` | string[] | Não | Bulk mode: multiple values for client_id |
| `priority_ids` | string[] | Não | Bulk mode: multiple values for priority_id |
| `status_ids` | string[] | Não | Bulk mode: multiple values for status_id |
| `services_catalogs_item_ids` | string[] | Não | Bulk mode: multiple values for services_catalogs_item_id |
| `requestor_ids` | string[] | Não | Bulk mode: multiple values for requestor_id |
| `responsible_ids` | string[] | Não | Bulk mode: multiple values for responsible_id |

#### `tiflux_create_ticket_answer`

Responde um chamado. Esta resposta É VISÍVEL PARA O SOLICITANTE e dispara notificação. Para uma nota que só a equipe vê, use tiflux_create_internal_communication. _(POST /api/tiflux/create/ticket/answer)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |
| `text` | string | Sim | Texto da resposta enviada ao solicitante. |
| `with_signature` | boolean | Não | Anexa a assinatura do atendente. |
| `files` | object[] | Não | Anexos (máximo 10 arquivos, 25MB cada). |

#### `tiflux_get_clients`

Detalha um ou vários clientes pelos ids, numa única chamada. _(POST /api/tiflux/get/clients)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `client_ids` | string[] | Sim | Ids dos clientes (1 a 50). |

#### `tiflux_get_ticket_stages_slas`

Histórico de estágios e SLAs de um chamado (quando entrou em cada estágio e como ficou o SLA). _(POST /api/tiflux/get/ticket/stages/slas)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Path: ticket_number |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |

#### `tiflux_get_tickets`

Detalha um ou vários chamados pelos números, numa única chamada. _(POST /api/tiflux/get/tickets)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_numbers` | string[] | Sim | Números dos chamados a detalhar (1 a 50). |
| `show_entities` | boolean | Não | Inclui as entidades (campos personalizados) do chamado. |
| `include_filled_entity` | boolean | Não | Inclui os valores preenchidos das entidades. |

#### `tiflux_list_appointments`

Lista apontamentos de horas de toda a organização por período, atendente e mesa. _(POST /api/tiflux/list/appointments)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `start_date` | string | Não | Data inicial (YYYY-MM-DD). |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |
| `user_ids` | string[] | Não | Filtra por atendentes. Máximo 15 ids por chamada (limite da API). |
| `desk_ids` | string[] | Não | Filtra por mesas. Máximo 15 ids por chamada (limite da API). |
| `include_valorization` | boolean | Não | Inclui a valorização financeira das horas apontadas. |

#### `tiflux_list_clients`

Lista clientes da organização, com busca parcial por nome. _(POST /api/tiflux/list/clients)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `name` | string | Não | Busca parcial por nome. |
| `active` | boolean | Não | Filtra por clientes ativos ou inativos. |
| `social_revenue` | string | Não | Filtra por razão social. |

#### `tiflux_list_contracts`

Lista os contratos de atendimento, com filtro por cliente, tipo e situação. _(POST /api/tiflux/list/contracts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `client_ids` | string[] | Não | Filtra por clientes. Máximo 15 ids por chamada (limite da API). |
| `contract_type_ids` | string[] | Não | Filtra por tipos de contrato. Máximo 15 ids por chamada (limite da API). |
| `status` | string | Não | Situação do contrato conforme a API oficial. |

#### `tiflux_list_departments`

Lista os departamentos da organização. _(POST /api/tiflux/list/departments)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `name` | string | Não | Busca parcial por nome. |

#### `tiflux_list_desk_priorities`

Lista as prioridades configuradas numa mesa, com os respectivos SLAs. _(POST /api/tiflux/list/desk/priorities)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `desk_id` | string | Sim | Path: desk_id |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |

#### `tiflux_list_desk_services_catalogs`

Lista os catálogos de serviços de uma mesa (a classificação do chamado). _(POST /api/tiflux/list/desk/services/catalogs)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `desk_id` | string | Sim | Path: desk_id |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |

#### `tiflux_list_desk_stages`

Lista os estágios (etapas do fluxo) de uma mesa. _(POST /api/tiflux/list/desk/stages)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `desk_id` | string | Sim | Path: desk_id |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |

#### `tiflux_list_desks`

Lista as mesas de atendimento, com busca parcial por nome. _(POST /api/tiflux/list/desks)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `name` | string | Não | Busca parcial por nome da mesa. |
| `active` | boolean | Não | Filtra por mesas ativas (padrão: ativas). |

#### `tiflux_list_internal_communications`

Lista as comunicações internas de um chamado (notas visíveis só para a equipe, nunca para o solicitante). _(POST /api/tiflux/list/internal/communications)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Path: ticket_number |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |

#### `tiflux_list_knowledges`

Busca artigos da base de conhecimento por texto e por pasta. _(POST /api/tiflux/list/knowledges)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `search` | string | Não | Busca textual no título e no conteúdo. |
| `knowledge_folder_ids` | string[] | Não | Filtra por pastas da base. Máximo 15 ids por chamada (limite da API). |

#### `tiflux_list_requestors`

Busca solicitantes por nome, e-mail ou telefone. _(POST /api/tiflux/list/requestors)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `name` | string | Não | Busca parcial por nome. |
| `email` | string | Não | Busca por e-mail. |
| `telephone` | string | Não | Busca por telefone. |
| `can_open_ticket` | boolean | Não | Só solicitantes com permissão de abrir chamado. |

#### `tiflux_list_technical_groups`

Lista os grupos de atendentes da organização. _(POST /api/tiflux/list/technical/groups)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |

#### `tiflux_list_technical_users`

Lista os atendentes, com filtro por nome, e-mail, mesa ou cliente. _(POST /api/tiflux/list/technical/users)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `name` | string | Não | Busca parcial por nome. |
| `email` | string | Não | Busca por e-mail. |
| `desk_id` | string | Não | Só atendentes da mesa. |
| `client_id` | string | Não | Só atendentes do cliente. |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |
| `client_ids` | string[] | Não | Bulk mode: multiple values for client_id |

#### `tiflux_list_ticket_answers`

Lista as respostas (comunicações visíveis ao solicitante) de um chamado. _(POST /api/tiflux/list/ticket/answers)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Path: ticket_number |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |

#### `tiflux_list_ticket_appointments`

Lista os apontamentos de horas de um chamado. _(POST /api/tiflux/list/ticket/appointments)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Path: ticket_number |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `user_id` | string | Não | Filtra por atendente. |
| `start_date` | string | Não | Data inicial (YYYY-MM-DD). |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |
| `user_ids` | string[] | Não | Bulk mode: multiple values for user_id |

#### `tiflux_list_ticket_files`

Lista os arquivos anexados a um chamado. _(POST /api/tiflux/list/ticket/files)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Path: ticket_number |
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |

#### `tiflux_list_tickets`

Lista chamados com filtros (situação, mesa, cliente, estágio, responsável, solicitante, período, SLA a vencer). _(POST /api/tiflux/list/tickets)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `filter_by` | string | Não | Situação dos chamados (padrão da API: abertos). (open, closed, canceled, all) |
| `desk_ids` | string[] | Não | Filtra por mesas. Máximo 15 ids por chamada (limite da API). |
| `client_ids` | string[] | Não | Filtra por clientes. Máximo 15 ids por chamada (limite da API). |
| `stage_ids` | string[] | Não | Filtra por estágios. Máximo 15 ids por chamada (limite da API). |
| `responsible_ids` | string[] | Não | Filtra por atendentes responsáveis. Máximo 15 ids por chamada (limite da API). |
| `requestor_ids` | string[] | Não | Filtra por solicitantes. Máximo 15 ids por chamada (limite da API). |
| `priority_ids` | string[] | Não | Filtra por prioridades. Máximo 15 ids por chamada (limite da API). |
| `services_catalogs_item_ids` | string[] | Não | Filtra por itens do catálogo de serviços. Máximo 15 ids por chamada (limite da API). |
| `requestor_email` | string | Não | E-mail do solicitante. |
| `date_type` | string | Não | Qual data o período filtra, ex.: created_at, closed_at. Combine com start_datetime e end_datetime. |
| `start_datetime` | string | Não | Início do período (ISO 8601). |
| `end_datetime` | string | Não | Fim do período (ISO 8601). |
| `sla_expiring_before` | string | Não | Só chamados com SLA vencendo antes desta data/hora (ISO 8601). |
| `group_by` | string | Não | Agrupamento do resultado conforme a API oficial. |

#### `tiflux_me`

Dados do usuário dono do token (nome, e-mail, perfil, feature flags). _(POST /api/tiflux/me)_

#### `tiflux_tickets_feedback_report`

Relatório de satisfação (feedback) dos chamados por período, com recorte por responsável, departamento ou grupo de atendentes. _(POST /api/tiflux/tickets/feedback/report)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `offset` | integer | Não | Página a retornar (1-based, padrão 1). Não é deslocamento de itens. |
| `limit` | integer | Não | Itens por página (1 a 200, padrão 20). |
| `start_date` | string | Não | Data inicial (YYYY-MM-DD). |
| `end_date` | string | Não | Data final (YYYY-MM-DD). |
| `tickets_list` | boolean | Não | Inclui a lista dos chamados avaliados, não só o consolidado. |
| `responsible_ids` | string[] | Não | Filtra por atendentes responsáveis. Máximo 15 ids por chamada (limite da API). |
| `department_ids` | string[] | Não | Filtra por departamentos. Máximo 15 ids por chamada (limite da API). |
| `technical_group_ids` | string[] | Não | Filtra por grupos de atendentes. Máximo 15 ids por chamada (limite da API). |

#### `tiflux_update_ticket`

Atualiza um chamado existente. _(POST /api/tiflux/update/ticket)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ticket_number` | string | Sim | Número do ticket (o identificador visível na tela do Tiflux). |
| `title` | string | Não |  |
| `description` | string | Não |  |
| `client_id` | string | Não |  |
| `desk_id` | string | Não | Transfere o chamado de mesa. |
| `priority_id` | string | Não |  |
| `priority_change_reason` | string | Não | Justificativa da mudança de prioridade, exigida por algumas configurações. |
| `status_id` | string | Não |  |
| `stage_id` | string | Não | Move o chamado de estágio (tiflux_list_desk_stages). |
| `services_catalogs_item_id` | string | Não |  |
| `requestor_id` | string | Não |  |
| `responsible_id` | string | Não | Transfere o chamado de responsável. |
| `followers` | string | Não |  |
| `client_ids` | string[] | Não | Bulk mode: multiple values for client_id |
| `desk_ids` | string[] | Não | Bulk mode: multiple values for desk_id |
| `priority_ids` | string[] | Não | Bulk mode: multiple values for priority_id |
| `status_ids` | string[] | Não | Bulk mode: multiple values for status_id |
| `stage_ids` | string[] | Não | Bulk mode: multiple values for stage_id |
| `services_catalogs_item_ids` | string[] | Não | Bulk mode: multiple values for services_catalogs_item_id |
| `requestor_ids` | string[] | Não | Bulk mode: multiple values for requestor_id |
| `responsible_ids` | string[] | Não | Bulk mode: multiple values for responsible_id |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_tiflux` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
