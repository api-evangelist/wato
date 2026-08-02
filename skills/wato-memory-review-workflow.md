---
name: Propose and merge reviewed team memory
description: Edit a Wato team's memory wiki on the draft branch, submit it for review, and merge the proposal into reviewed main memory.
api: mcp/wato-mcp.yml
operations:
- wato_get_usage_guide
- mesh_search_memory
- mesh_read_memory_file
- mesh_update_memory_branch_from_main
- mesh_write_memory_file
- mesh_patch_memory_file
- mesh_submit_memory_branch_for_review
- mesh_list_memory_proposals
- mesh_get_memory_proposal
- mesh_merge_memory_proposal
generated: '2026-07-21'
method: generated
---

# Propose and merge reviewed team memory

Wato memory is a git-like Markdown wiki: agents write only to a draft branch, and
changes reach the team's reviewed main memory through a proposal review. Every
call is traced (tool, user, team, input, result, timestamp).

## Steps

1. **Orient.** Call `wato_get_usage_guide` for team info and your memory
   permissions; use `mesh_search_memory` / `mesh_read_memory_file` to find the
   files you intend to change (reads come from reviewed main).
2. **Sync the draft branch.** Call `mesh_update_memory_branch_from_main` so your
   draft starts from the latest reviewed memory and merges stay clean.
3. **Edit on the draft branch.** Use `mesh_patch_memory_file` for targeted
   find-and-replace edits, or `mesh_write_memory_file` to write a file's full
   contents. Writes never touch reviewed main directly.
4. **Submit for review.** Call `mesh_submit_memory_branch_for_review` to turn the
   draft branch into a memory proposal (states: draft → submitted → pending
   review → ready to merge → merged, or has-conflicts).
5. **Review and merge.** List with `mesh_list_memory_proposals`, inspect the diff
   with `mesh_get_memory_proposal`, refresh a stale one with
   `mesh_refresh_memory_proposal`, then `mesh_merge_memory_proposal` once clean
   (merge requires review access).

## Rules

- Never try to bypass review: there is no direct write to reviewed main.
- Resolve `has-conflicts` by refreshing the proposal from main and re-editing.
- Audit history is available via `mesh_list_memory_changes` and
  `mesh_get_memory_change` (before/after Markdown).
