---
name: Publish a live dashboard artifact
description: Author an HTML artifact that loads fresh data through Wato-approved connectors and bind it to a server-side live view policy.
api: mcp/wato-mcp.yml
operations:
- wato_get_live_artifact_primitives
- wato_bind_dashboard_view
generated: '2026-07-21'
method: generated
---

# Publish a live dashboard artifact

Live dashboards are hosted HTML artifacts that load fresh data at runtime
through Wato-approved connectors, with credentials kept server-side — the
browser never sees connector secrets, OAuth tokens, or MCP credentials.

## Steps

1. **Get the contract.** Call `wato_get_live_artifact_primitives` for the current
   authoring contract and `window.WatoDashboard` SDK details before writing any
   HTML.
2. **Author.** Mark live sections with `data-wato-id` and `data-wato-call`
   attributes declaring the connector calls the page needs; in script, use
   `const wato = await window.WatoDashboard.init()` then `wato.call(...)`,
   `wato.callTool(connector, tool, args)`, or `wato.callAction(...)`, and
   `refreshSession` on token expiry.
3. **Bind the policy.** Call `wato_bind_dashboard_view` with `viewSlug`, `title`,
   the `html`, and `allowedOrigins`. Wato scans the static `data-wato-call`
   declarations against the team's connector/tool registry and returns
   `status: "active"` for approved reads or `review_required` for
   writes/destructive calls that need admin approval.
4. **Serve over HTTP(S).** Live dashboards never run from `file://`; org/team are
   derived from the hosted URL (or provided explicitly for localhost).

## Rules

- Declare every connector call statically — undeclared calls are not approved at
  runtime.
- Expect `review_required` for anything non-read; get an admin approval before
  shipping the view.
