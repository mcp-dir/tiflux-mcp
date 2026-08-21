# Tiflux

### Tiflux for Claude, ChatGPT and AI agents

Wrapper for the official Tiflux API v2 (help desk and service desk): tickets with replies to the requester, internal notes, attachments, stage and SLA history, time entries, clients and requesters, desks with stages, priorities and service catalog, knowledge base, contracts and billing and satisfaction reports. Read and write. Authenticated by the user's API Session token.

- 📊 **30 tools**
- ✏️ **Read and write**
- 💬 **Works with any MCP client**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Magic-link login (no password)**

[Portuguese version](README.md) · [Full docs (PT-BR)](docs/)

---

## One-click install

### Claude (Web and Desktop)

[➕ Open in Claude and connect](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

Manual: [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Add custom connector** → name `Tiflux`, URL `https://api.mcp.ai/p_tiflux`.

### Cursor

[➕ Install in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=tiflux&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF90aWZsdXgifQ==)

### VS Code (Copilot Chat)

[➕ Install in VS Code](vscode:mcp/install?name=tiflux&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_tiflux%22%7D)

### Any other MCP-over-HTTP client

```
https://api.mcp.ai/p_tiflux
```

---

## 30 tools

| Tool | Description |
|---|---|
| `tiflux_me` | Dados do usuário dono do token (nome, e-mail, perfil, feature flags). |
| `tiflux_list_tickets` | Lista chamados com filtros (situação, mesa, cliente, estágio, responsável, solicitante, período, SLA a vencer). |
| `tiflux_get_tickets` | Detalha um ou vários chamados pelos números, numa única chamada. |
| `tiflux_create_ticket` | Abre um novo chamado. Resolva desk_id e client_id antes com tiflux_list_desks e tiflux_list_clients. Identifique o solicitante por requestor_id, ou pelos campos requestor_name e requestor_email quando ele ainda não ex… |
| `tiflux_update_ticket` | Atualiza um chamado existente. |
| `tiflux_close_ticket` | Encerra um chamado, marcando-o como resolvido. |
| `tiflux_cancel_ticket` | Cancela um chamado, encerrando-o SEM tratá-lo como atendido (duplicado, aberto por engano, fora de escopo). |
| `tiflux_list_ticket_answers` | Lista as respostas (comunicações visíveis ao solicitante) de um chamado. |
| `tiflux_create_ticket_answer` | Responde um chamado. Esta resposta É VISÍVEL PARA O SOLICITANTE e dispara notificação. Para uma nota que só a equipe vê, use tiflux_create_internal_communication. |
| `tiflux_list_internal_communications` | Lista as comunicações internas de um chamado (notas visíveis só para a equipe, nunca para o solicitante). |
| `tiflux_create_internal_communication` | Cria uma comunicação interna num chamado. |
| `tiflux_list_ticket_files` | Lista os arquivos anexados a um chamado. |
| `tiflux_get_ticket_stages_slas` | Histórico de estágios e SLAs de um chamado (quando entrou em cada estágio e como ficou o SLA). |
| `tiflux_list_ticket_appointments` | Lista os apontamentos de horas de um chamado. |
| `tiflux_create_appointment` | Lança um apontamento de horas num chamado, em nome do usuário dono do token. |
| `tiflux_list_appointments` | Lista apontamentos de horas de toda a organização por período, atendente e mesa. |
| `tiflux_list_clients` | Lista clientes da organização, com busca parcial por nome. |
| `tiflux_get_clients` | Detalha um ou vários clientes pelos ids, numa única chamada. |
| `tiflux_list_requestors` | Busca solicitantes por nome, e-mail ou telefone. |
| `tiflux_list_desks` | Lista as mesas de atendimento, com busca parcial por nome. |
| `tiflux_list_desk_stages` | Lista os estágios (etapas do fluxo) de uma mesa. |
| `tiflux_list_desk_priorities` | Lista as prioridades configuradas numa mesa, com os respectivos SLAs. |
| `tiflux_list_desk_services_catalogs` | Lista os catálogos de serviços de uma mesa (a classificação do chamado). |
| `tiflux_list_technical_users` | Lista os atendentes, com filtro por nome, e-mail, mesa ou cliente. |
| `tiflux_list_technical_groups` | Lista os grupos de atendentes da organização. |
| `tiflux_list_departments` | Lista os departamentos da organização. |
| `tiflux_list_knowledges` | Busca artigos da base de conhecimento por texto e por pasta. |
| `tiflux_list_contracts` | Lista os contratos de atendimento, com filtro por cliente, tipo e situação. |
| `tiflux_billings_history` | Histórico de faturamentos. Os pares de data são obrigatórios em conjunto: billing_start_date com billing_end_date, e due_start_date com due_end_date. Bulk support: accepts client_ids for batched execution. |
| `tiflux_tickets_feedback_report` | Relatório de satisfação (feedback) dos chamados por período, com recorte por responsável, departamento ou grupo de atendentes. |

---

## Pricing

See [docs/precos.md](docs/precos.md) (PT-BR).

---

## License

MIT — see [LICENSE](LICENSE). The MCP server at `api.mcp.ai/p_tiflux` is proprietary (hosted); this repo (manifests, docs, skills) is MIT.
