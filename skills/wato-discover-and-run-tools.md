---
name: Discover and run approved connector tools
description: Find the connector tools a Wato team has approved, fetch their input schemas, and execute them through the governed MCP gateway.
api: mcp/wato-mcp.yml
operations:
- wato_get_usage_guide
- wato_search_agent_context
- mesh_search_tools
- mesh_get_tool_schemas
- mesh_execute_tool
generated: '2026-07-21'
method: generated
---

# Discover and run approved connector tools

The Wato gateway (`https://mesh.watolabs.com/api/mcp`, Streamable HTTP + OAuth,
team-scoped via `?team=<slug>` or the `x-mesh-team` header) exposes only the
connector tools approved for your team and role. Credentials stay server-side;
you never see connector secrets or OAuth tokens.

## Steps

1. **Orient.** Call `wato_get_usage_guide` first — it returns the team, the
   recommended calls, and best practices. Use `wato_search_agent_context` to pull
   the team's rules, skills, and authorized memory summaries relevant to the task.
2. **Discover.** Call `mesh_search_tools` with a name, connector, description,
   regex, or schema query to find candidate tools (e.g. Datadog or Notion
   connector tools).
3. **Get the contract.** Call `mesh_get_tool_schemas` for the exact input schema
   of each tool you plan to use — do not guess parameters.
4. **Execute.** Call `mesh_execute_tool` with the tool name and schema-valid
   arguments. Only approved tools run; approval is governed per workspace, team,
   and role.

## Rules

- Every execution is recorded as an auditable trace (tool, user, team, input,
  result, timestamp) — assume everything you run is reviewed.
- If a tool is missing, it likely is not approved for the team; ask an admin to
  approve the connector rather than working around it.
