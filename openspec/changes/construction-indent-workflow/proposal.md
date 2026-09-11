## Why

Construction sites run on indents (site demand notes), but generic inventory tools like Zoho Inventory model warehouses selling to customers — there is no demand-note primitive, no schedule-linked needed-by date, and no site/store fulfilment split. This change introduces the indent workflow as the core of the construction inventory app.

## What Changes

- Indent lifecycle with states: DRAFT → PENDING → APPROVED → FULFILLED / PART-FULFILLED → CLOSED, plus REJECTED and CANCELLED with mandatory reason codes.
- Single-level approval: site engineer submits, project manager approves or rejects. Overdue pending indents surface a slab-risk nudge (no auto-approval).
- Schedule-driven needed-by dates: each indent line carries a needed-by date plus activity reference (e.g. Block B slab); pending queue sorts by needed-by date.
- Manual fulfilment split per line: after approval, each line is fulfilled via store issue, yard transfer, or purchase demand; partials leave an explicit balance in a human-decided bucket (raise PO, re-issue, or short-close with reason). No auto-PO.
- Site receipt close-out: store issue is not receipt; site confirms quantity received with receiver identity (vehicle/gate-pass recorded at issue).
- Retro-entry quarantine: late/paperwork-after-fact indents are entered only via an admin screen, flagged as retroactive with a required reason, and excludable from lead-time reports.

## Capabilities

### New Capabilities

- `indent-lifecycle`: indent states, transitions, single-level engineer→PM approval, rejection/cancellation reason codes, needed-by/activity scheduling fields.
- `indent-fulfilment`: per-line fulfilment split (store issue, transfer, purchase demand), partial-balance decision bucket, short-close rules, site receipt confirmation.
- `exception-logging`: retroactive indent entry via admin screen, retro flagging, reason capture, report exclusion rules.

### Modified Capabilities

- None (greenfield; `openspec/specs/` is empty).

## Impact

- No existing code, APIs, or systems affected (new project, no implementation yet).
- Establishes the contract for follow-up specs, design (roles, queues, offline/photo-challan considerations), and tasks.
- Non-goals for this change: BOQ linkage, valuation/costing, vendor/PO execution detail, barcode/offline sync — noted as later layers.
