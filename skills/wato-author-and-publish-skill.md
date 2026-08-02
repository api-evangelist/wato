---
name: Author and publish a Wato skill
description: Draft a versioned team skill (SKILL.md plus bundled files) over MCP, move it through review, and publish it for agents to load.
api: mcp/wato-mcp.yml
operations:
- wato_list_skills
- wato_get_skill
- wato_get_skill_file
- wato_save_skill_draft
- wato_publish_skill
generated: '2026-07-21'
method: generated
---

# Author and publish a Wato skill

Wato skills are reusable, versioned units of capability — a SKILL.md plus
optional bundled files (scripts, reference docs, templates) — that agents
discover and load. Lifecycle: draft → proposed → published, with permissions
gating each step.

## Steps

1. **Check what exists.** Call `wato_list_skills` and `wato_get_skill` (by id or
   slug) to avoid duplicating a published skill; inspect bundled files with
   `wato_get_skill_file`.
2. **Draft.** Call `wato_save_skill_draft` with the skill Markdown; the same call
   can set bundled files so scripts and reference docs ship alongside the skill
   and are loaded only when needed.
3. **Iterate.** Re-call `wato_save_skill_draft` to update the draft as you refine
   the instructions.
4. **Publish.** Call `wato_publish_skill` by id/slug once the draft is approved —
   publishing is permission-gated and produces a new version agents can load.

## Rules

- Keep the SKILL.md focused on one workflow; push bulk reference material into
  bundled files.
- Published skills are versioned — publishing again creates a new version rather
  than silently mutating the old one.
