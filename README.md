# Tiflux

### Tiflux para Claude, ChatGPT e agentes de IA

Wrapper da API oficial v2 do Tiflux (help desk e service desk): chamados com respostas ao solicitante, notas internas, anexos, histórico de estágios e SLAs, apontamentos de horas, clientes e solicitantes, mesas com estágios, prioridades e catálogo de serviços, base de conhecimento, contratos e relatórios de faturamento e satisfação. Consulta e operação. Autenticação por token de Sessão API do usuário.

- 📊 **30 ferramentas**
- ✏️ **Leitura e escrita**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Adicionar conector personalizado** → cole **Nome** `Tiflux` e **URL** `https://api.mcp.ai/p_tiflux`.

### Cursor

[➕ Instalar Tiflux no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=tiflux&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF90aWZsdXgifQ==)

### VS Code (Copilot Chat)

[➕ Instalar Tiflux no VS Code](vscode:mcp/install?name=tiflux&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_tiflux%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_tiflux
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Liste os chamados abertos com SLA vencendo nas próximas 4 horas
Abra um chamado na mesa de Suporte para o cliente X sobre a impressora fora do ar
Responda o chamado 1234 avisando que a correção foi aplicada e encerre
Quantas horas foram apontadas por atendente no mês passado
```

---

## 30 ferramentas disponíveis

| Tool | Descrição |
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

Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Planos a partir do tier grátis. Veja [docs/precos.md](docs/precos.md).

---

## Privacidade & LGPD

- **Sub-processadores**: Tiflux Sistema de Gestão, o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_tiflux`.


---

## Suporte

- 📧 [tiflux@mcp.ai](mailto:tiflux@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/tiflux-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_tiflux` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
