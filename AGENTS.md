# AGENTS.md — construct

OpenSpec-first repo. Every implementation follows the spec lifecycle; no exceptions beyond typos and single-line fixes.

## Workflow (Oh My Pi spells it `/opsx-*`; plain `/openspec-*` skill names also work)

1. Explore: `/opsx-explore <idea>` — thinking only, no code, no files.
2. Propose: `/opsx-propose <description>` — writes `openspec/changes/<name>/` (`proposal.md`, `specs/`, `design.md`, `tasks.md`). Planning only; stop after presenting artifacts.
3. Review: user reads `proposal.md` → `specs/` → `tasks.md` first. Fix plan as words, not code.
4. Apply: `/opsx-apply <change-name>` in a fresh session; work `tasks.md` top to bottom, checking boxes. Re-run to resume.
5. Archive: `/opsx-archive <change-name>` when every box is checked — specs absorb the change, folder moves to `openspec/changes/archive/`.

Shortcut: `/construct <request>` routes to the right step.

## Rules

- Never write code without an approved change folder behind it.
- `openspec/specs/` is the source of truth for what the system does; `openspec/changes/archive/` is how it got there. Commit both with the code.
- Verify every behavioral change by running it before yielding.
