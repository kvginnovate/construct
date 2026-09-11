## Context

Greenfield project: no existing code, no data to migrate, `openspec/specs/` is empty. The specs in this change (`indent-lifecycle`, `indent-fulfilment`, `exception-logging`) are the behavior contract; this doc records how to structure the implementation. See `proposal.md` for motivation.

Key constraints from exploration: exactly two approval actors (engineer, PM); needed-by dates are schedule-driven and real; partial balances are always human-decided (never auto-PO); retro-entry is quarantined but must stay honest; site connectivity is unreliable, so flows must tolerate delayed sync later (offline itself is a non-goal here).

## Goals / Non-Goals

**Goals:**

- One indent record carrying its full lifecycle: states, per-line fulfilment legs, receipt confirmations, and retro flag in a single auditable trail.
- Three role-shaped queues that fall out of the state machine without extra bookkeeping: engineer (my drafts/pending), PM (pending sorted by needed-by), buyer/store (decision bucket + purchase legs).
- Fulfilment legs as the single quantity ledger so issue, transfer, purchase, receipt, shortage, and short-close never double-count.

**Non-Goals:**

- Offline sync, photo-challan capture, barcode scanning — noted as the next layer, shaped for but not built here.
- BOQ linkage, valuation/costing, vendor/PO execution detail — explicitly deferred per proposal.
- Notification channels (push/SMS/WhatsApp) for the slab-risk nudge — the nudge is a visible state, delivery mechanism undecided.

## Decisions

### 1. State machine + per-line leg ledger as the core model
Model the indent header as a finite state machine (DRAFT → PENDING → APPROVED → FULFILLED / PART-FULFILLED → CLOSED, with REJECTED / CANCELLED terminals) and each line as a ledger of fulfilment legs (issue / transfer / purchase), each leg carrying its own fulfilled, received, and short-closed quantities. Header state derives from line/leg aggregates rather than being set independently.
*Alternative considered:* header status as a manually-set field. Rejected — derived state cannot drift from leg reality, which is where theft/shortage disputes live.

### 2. Decision bucket as a first-class queue, not a status
Unresolved balances live as queryable rows (indent + line + balance qty + age + needed-by) instead of relying on PART-FULFILLED status alone. The buyer's morning screen reads this queue directly; resolving a row attaches a new leg or a short-close record.
*Alternative considered:* status-only with ad-hoc balance math per view. Rejected — balances are the buyer's actual workload and deserve a stable, countable surface.

### 3. Receipt as a separate event on the leg, never implied by issue
Issue and receipt are two events on the same leg: issue records vehicle/gate-pass + issued qty; receipt records received qty + receiver identity. Fulfilment progress counts received (or short-closed), not issued, so the issue↔receipt gap stays visible by construction.
*Alternative considered:* single "fulfilled qty" updated at issue, receipt optional. Rejected — collapses exactly the gap where site shortage and pilferage hide.

### 4. Retro quarantine at the entry boundary, not the data layer
Retro indents are the same record shape plus a persistent flag and late-reason, but creatable only through the admin path. Reports filter on the flag (exclude by default). No separate table, no divergent lifecycle.
*Alternative considered:* separate retro table. Rejected — splits the audit trail and doubles every query; a flag preserves one trail with honest provenance.

### 5. Nudge as derived visibility with a configurable risk window
The slab-risk nudge derives from needed-by date vs. now (configurable window, e.g. 48h) and pending status. No timers, no background jobs, no auto-transitions — a pure function of stored dates evaluated at read time.
*Alternative considered:* scheduled escalation job. Rejected — adds infrastructure for v1; the queue sort (earliest needed-by first) already surfaces urgency, the nudge just labels it.

## Risks / Trade-offs

- [Risk] PM becomes the bottleneck with single-level approval and no auto-escalation → Mitigation: pending queue sorted by needed-by plus risk nudge makes stall visible; escalation rules deferred to a later change once real latency data exists (retro entries excluded so data stays clean).
- [Risk] Per-line activity references stay free text and never become schedule-linkable → Mitigation: activity reference is a structured field from day one (name + date), so a later schedule import binds to existing data instead of requiring re-entry.
- [Risk] UoM mismatch (indent in trucks, issue in cft, bill in brass) breaks leg quantity math → Mitigation: legs record their own UoM with the line's canonical UoM; conversion factors are a known gap flagged for the UoM change, and legs block when units are incompatible rather than silently coercing.
- [Risk] Retro flag stigmatizes legitimate emergency practice and drives it off-system → Mitigation: retro path is frictionless (one reason field), never blocks, and the per-engineer counter is admin-visible only, not a punishment surface.

## Migration Plan

Not applicable — greenfield, no existing data or deployments. First implementation creates the model directly from these specs.

## Open Questions

- Risk-window duration for the slab nudge (24h? 48h? per-material lead time?) — configurable constant; safe to pick a default and tune from usage.
- Rolling window length for the per-engineer retro counter (30d? 90d?) — display-only signal, tunable without spec changes.
- Activity reference vocabulary (free text vs. per-site activity list) — field shape is fixed; vocabulary control can tighten later without breaking the spec.
