---
description: "Build anything in this repo the OpenSpec way: explore, propose, review, apply, archive. Use for every implementation task."
---

# Construct

Every non-trivial change in this repo goes through the OpenSpec lifecycle.
No direct code edits for anything beyond a typo or single-line fix.

Route by request type:

- Vague idea / unfamiliar area → `/opsx-explore <idea>`, then `/opsx-propose`
- Clear change → `/opsx-propose <description>` (planning only, no code)
- Approved plan → `/opsx-apply <change-name>` (fresh session, task by task)
- Plan needs fixing mid-flight → `/opsx-update <change-name>`
- Done, all tasks checked → `/opsx-archive <change-name>`

Rules:

- `propose` authorizes planning only. Never implement in the same turn.
- User must review `proposal.md` → `specs/` → `tasks.md` before `apply`.
- Resume interrupted work by re-running `/opsx-apply <change-name>`; progress is the `tasks.md` checkboxes.
- Verify behavior before yielding (run it, per repo `AGENTS.md`).
- Change state lives under `openspec/changes/`; truth lives under `openspec/specs/`.

## Invoke

```text
/construct $1        # $1: what to build, fix, or change
```
