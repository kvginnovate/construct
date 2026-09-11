## 1. Indent lifecycle

- [ ] 1.1 Implement indent record with DRAFT → PENDING → APPROVED → FULFILLED / PART-FULFILLED → CLOSED transitions plus REJECTED and CANCELLED terminals, header state derived from line/leg aggregates.
- [ ] 1.2 Implement engineer submit and single-level PM approve/reject, rejecting approval attempts from non-PM roles.
- [ ] 1.3 Require reason codes on rejection and cancellation and keep them visible on the indent record.
- [ ] 1.4 Add per-line needed-by date and activity reference; sort the PM pending queue by earliest needed-by first.
- [ ] 1.5 Derive the slab-risk nudge from needed-by vs. now with a configurable risk window; verify no path auto-approves an indent.

## 2. Fulfilment and receipt

- [ ] 2.1 Implement per-line fulfilment legs (store issue, yard transfer, purchase demand) with per-leg quantity tracking that blocks over-fulfilment.
- [ ] 2.2 Implement the balance decision bucket queue; resolve balances via purchase demand, re-issue, or short-close, with no auto-PO path.
- [ ] 2.3 Implement short-close with mandatory reason, freezing the line and recording short-closed quantity distinctly.
- [ ] 2.4 Implement site receipt confirmation (received qty + receiver identity) as the event that advances fulfilment, keeping the issue↔receipt gap visible.
- [ ] 2.5 Record vehicle and gate-pass details at issue and surface them on the receipt screen.

## 3. Retro-entry quarantine

- [ ] 3.1 Restrict retroactive entry to the admin screen with a required late-reason; keep the normal engineer flow free of any retro path.
- [ ] 3.2 Persist the retro flag and display it on indent detail, queue rows, and fulfilment legs.
- [ ] 3.3 Exclude retroactive indents from lead-time reports by default behind an explicit opt-in.
- [ ] 3.4 Maintain the per-engineer rolling retro counter as an admin-visible signal that never blocks submission.

## 4. Verification

- [ ] 4.1 Walk the full flow end to end: draft → submit → approve → split fulfil → partial balance decision → receipt → close, plus reject, cancel, and short-close paths.
- [ ] 4.2 Verify report exclusion and retro-flag visibility across queues with mixed normal and retroactive data.
