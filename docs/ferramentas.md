# Ferramentas

Tiflux expõe 30 ferramentas.

### 1. `tiflux_me`
**Input**: nenhum input

Dados do usuário dono do token (nome, e-mail, perfil, feature flags).

### 2. `tiflux_list_tickets`
**Input**: `offset` (opcional), `limit` (opcional), `filter_by` (opcional), `desk_ids` (opcional), `client_ids` (opcional), `stage_ids` (opcional), `responsible_ids` (opcional), `requestor_ids` (opcional), `priority_ids` (opcional), `services_catalogs_item_ids` (opcional), `requestor_email` (opcional), `date_type` (opcional), `start_datetime` (opcional), `end_datetime` (opcional), `sla_expiring_before` (opcional), `group_by` (opcional)

Lista chamados com filtros (situação, mesa, cliente, estágio, responsável, solicitante, período, SLA a vencer).

### 3. `tiflux_get_tickets`
**Input**: `ticket_numbers`, `show_entities` (opcional), `include_filled_entity` (opcional)

Detalha um ou vários chamados pelos números, numa única chamada.

### 4. `tiflux_create_ticket`
**Input**: `title`, `description`, `desk_id` (opcional), `client_id` (opcional), `priority_id` (opcional), `status_id` (opcional), `services_catalogs_item_id` (opcional), `requestor_id` (opcional), `requestor_name` (opcional), `requestor_email` (opcional), `requestor_telephone` (opcional), `responsible_id` (opcional), `followers` (opcional), `parent_ticket_number` (opcional), `files` (opcional), `desk_ids` (opcional), `client_ids` (opcional), `priority_ids` (opcional), `status_ids` (opcional), `services_catalogs_item_ids` (opcional), `requestor_ids` (opcional), `responsible_ids` (opcional)

Abre um novo chamado. Resolva desk_id e client_id antes com tiflux_list_desks e tiflux_list_clients. Identifique o solicitante por requestor_id, ou pelos campos requestor_name e requestor_email quando ele ainda não ex…

### 5. `tiflux_update_ticket`
**Input**: `ticket_number`, `title` (opcional), `description` (opcional), `client_id` (opcional), `desk_id` (opcional), `priority_id` (opcional), `priority_change_reason` (opcional), `status_id` (opcional), `stage_id` (opcional), `services_catalogs_item_id` (opcional), `requestor_id` (opcional), `responsible_id` (opcional), `followers` (opcional), `client_ids` (opcional), `desk_ids` (opcional), `priority_ids` (opcional), `status_ids` (opcional), `stage_ids` (opcional), `services_catalogs_item_ids` (opcional), `requestor_ids` (opcional), `responsible_ids` (opcional)

Atualiza um chamado existente.

### 6. `tiflux_close_ticket`
**Input**: `ticket_number`

Encerra um chamado, marcando-o como resolvido.

### 7. `tiflux_cancel_ticket`
**Input**: `ticket_number`

Cancela um chamado, encerrando-o SEM tratá-lo como atendido (duplicado, aberto por engano, fora de escopo).

### 8. `tiflux_list_ticket_answers`
**Input**: `ticket_number`, `offset` (opcional), `limit` (opcional)

Lista as respostas (comunicações visíveis ao solicitante) de um chamado.

### 9. `tiflux_create_ticket_answer`
**Input**: `ticket_number`, `text`, `with_signature` (opcional), `files` (opcional)

Responde um chamado. Esta resposta É VISÍVEL PARA O SOLICITANTE e dispara notificação. Para uma nota que só a equipe vê, use tiflux_create_internal_communication.

### 10. `tiflux_list_internal_communications`
**Input**: `ticket_number`, `offset` (opcional), `limit` (opcional)

Lista as comunicações internas de um chamado (notas visíveis só para a equipe, nunca para o solicitante).

### 11. `tiflux_create_internal_communication`
**Input**: `ticket_number`, `text`, `files` (opcional)

Cria uma comunicação interna num chamado.

### 12. `tiflux_list_ticket_files`
**Input**: `ticket_number`, `offset` (opcional), `limit` (opcional)

Lista os arquivos anexados a um chamado.

### 13. `tiflux_get_ticket_stages_slas`
**Input**: `ticket_number`, `offset` (opcional), `limit` (opcional)

Histórico de estágios e SLAs de um chamado (quando entrou em cada estágio e como ficou o SLA).

### 14. `tiflux_list_ticket_appointments`
**Input**: `ticket_number`, `offset` (opcional), `limit` (opcional), `user_id` (opcional), `start_date` (opcional), `end_date` (opcional), `user_ids` (opcional)

Lista os apontamentos de horas de um chamado.

### 15. `tiflux_create_appointment`
**Input**: `ticket_number`, `date`, `init_time`, `end_time`, `description`

Lança um apontamento de horas num chamado, em nome do usuário dono do token.

### 16. `tiflux_list_appointments`
**Input**: `offset` (opcional), `limit` (opcional), `start_date` (opcional), `end_date` (opcional), `user_ids` (opcional), `desk_ids` (opcional), `include_valorization` (opcional)

Lista apontamentos de horas de toda a organização por período, atendente e mesa.

### 17. `tiflux_list_clients`
**Input**: `offset` (opcional), `limit` (opcional), `name` (opcional), `active` (opcional), `social_revenue` (opcional)

Lista clientes da organização, com busca parcial por nome.

### 18. `tiflux_get_clients`
**Input**: `client_ids`

Detalha um ou vários clientes pelos ids, numa única chamada.

### 19. `tiflux_list_requestors`
**Input**: `offset` (opcional), `limit` (opcional), `name` (opcional), `email` (opcional), `telephone` (opcional), `can_open_ticket` (opcional)

Busca solicitantes por nome, e-mail ou telefone.

### 20. `tiflux_list_desks`
**Input**: `offset` (opcional), `limit` (opcional), `name` (opcional), `active` (opcional)

Lista as mesas de atendimento, com busca parcial por nome.

### 21. `tiflux_list_desk_stages`
**Input**: `desk_id`, `offset` (opcional), `limit` (opcional), `desk_ids` (opcional)

Lista os estágios (etapas do fluxo) de uma mesa.

### 22. `tiflux_list_desk_priorities`
**Input**: `desk_id`, `offset` (opcional), `limit` (opcional), `desk_ids` (opcional)

Lista as prioridades configuradas numa mesa, com os respectivos SLAs.

### 23. `tiflux_list_desk_services_catalogs`
**Input**: `desk_id`, `offset` (opcional), `limit` (opcional), `desk_ids` (opcional)

Lista os catálogos de serviços de uma mesa (a classificação do chamado).

### 24. `tiflux_list_technical_users`
**Input**: `offset` (opcional), `limit` (opcional), `name` (opcional), `email` (opcional), `desk_id` (opcional), `client_id` (opcional), `desk_ids` (opcional), `client_ids` (opcional)

Lista os atendentes, com filtro por nome, e-mail, mesa ou cliente.

### 25. `tiflux_list_technical_groups`
**Input**: `offset` (opcional), `limit` (opcional)

Lista os grupos de atendentes da organização.

### 26. `tiflux_list_departments`
**Input**: `offset` (opcional), `limit` (opcional), `name` (opcional)

Lista os departamentos da organização.

### 27. `tiflux_list_knowledges`
**Input**: `offset` (opcional), `limit` (opcional), `search` (opcional), `knowledge_folder_ids` (opcional)

Busca artigos da base de conhecimento por texto e por pasta.

### 28. `tiflux_list_contracts`
**Input**: `offset` (opcional), `limit` (opcional), `client_ids` (opcional), `contract_type_ids` (opcional), `status` (opcional)

Lista os contratos de atendimento, com filtro por cliente, tipo e situação.

### 29. `tiflux_billings_history`
**Input**: `offset` (opcional), `limit` (opcional), `billing_start_date` (opcional), `billing_end_date` (opcional), `due_start_date` (opcional), `due_end_date` (opcional), `client_id` (opcional), `nfe_number` (opcional), `ticket_number` (opcional), `type` (opcional), `client_ids` (opcional)

Histórico de faturamentos. Os pares de data são obrigatórios em conjunto: billing_start_date com billing_end_date, e due_start_date com due_end_date. Bulk support: accepts client_ids for batched execution.

### 30. `tiflux_tickets_feedback_report`
**Input**: `offset` (opcional), `limit` (opcional), `start_date` (opcional), `end_date` (opcional), `tickets_list` (opcional), `responsible_ids` (opcional), `department_ids` (opcional), `technical_group_ids` (opcional)

Relatório de satisfação (feedback) dos chamados por período, com recorte por responsável, departamento ou grupo de atendentes.

## Prompts de exemplo

```
Liste os chamados abertos com SLA vencendo nas próximas 4 horas
Abra um chamado na mesa de Suporte para o cliente X sobre a impressora fora do ar
Responda o chamado 1234 avisando que a correção foi aplicada e encerre
Quantas horas foram apontadas por atendente no mês passado
```
