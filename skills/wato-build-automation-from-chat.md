---
name: Turn a chat into a reusable automation
description: Distill the connector actions taken in a conversation into a Wato automation draft, then inspect and refine it over MCP.
api: mcp/wato-mcp.yml
operations:
- wato_create_automation_from_chat
- wato_get_automation_draft
- wato_edit_automation_draft
generated: '2026-07-21'
method: generated
---

# Turn a chat into a reusable automation

After an agent has performed real connector actions in a conversation, Wato can
distill those actions into a triggered automation draft on the Wato canvas
(lifecycle: draft → submitted for review → live).

## Steps

1. **Distill.** Call `wato_create_automation_from_chat` at the end of a session
   in which you executed connector tools — it turns the actions taken into a
   reusable automation draft.
2. **Inspect.** Call `wato_get_automation_draft` to read the draft's steps in
   flow order with field tagging; verify each step matches what should be
   automated.
3. **Refine.** Call `wato_edit_automation_draft` with a plain-English instruction
   (e.g. change a filter, add a notification step) and re-inspect until right.
4. **Hand off for publish.** Publishing/testing the triggered workflow happens on
   the Wato canvas (see https://docs.watolabs.com/docs/quickstart-automation);
   automation drafts go through review before running live.

## Rules

- Build the automation from actions that actually happened — the draft mirrors
  the traced tool calls, so run the flow manually first.
- Plan limits cap automations (Free 3 / Personal 25 / Team 100 — see
  plans/wato-plans.yml).
