## Purpose

Defines the indent demand-note lifecycle for construction sites, from draft through single-level approval to closure, with schedule-driven needed-by dates.

## ADDED Requirements

### Requirement: Indent lifecycle states and transitions
The system SHALL support indent states DRAFT, PENDING, APPROVED, FULFILLED, PART-FULFILLED, CLOSED, REJECTED, and CANCELLED, with transitions DRAFT → PENDING → APPROVED → FULFILLED / PART-FULFILLED → CLOSED, PENDING → REJECTED, and DRAFT / PENDING → CANCELLED.

#### Scenario: Standard happy-path transition
- **WHEN** an engineer submits a DRAFT indent
- **THEN** the indent becomes PENDING, and after PM approval it becomes APPROVED, and once all lines are fully fulfilled and receipted it becomes CLOSED.

#### Scenario: Partial fulfilment state
- **WHEN** an APPROVED indent has at least one line partially fulfilled and none over-fulfilled
- **THEN** the indent state SHALL be PART-FULFILLED until all balances are resolved.

### Requirement: Single-level engineer to PM approval
The system SHALL allow only two approval actors: the site engineer submits, and the project manager approves or rejects. No additional approval levels SHALL be required for an indent to become APPROVED.

#### Scenario: PM approves pending indent
- **WHEN** a PENDING indent is approved by the project manager
- **THEN** the indent becomes APPROVED and its lines become eligible for fulfilment.

#### Scenario: Non-PM approval attempt
- **WHEN** a user without the project-manager role attempts to approve a PENDING indent
- **THEN** the system SHALL reject the action and leave the indent PENDING.

### Requirement: Mandatory reason codes for rejection and cancellation
The system SHALL require a reason code whenever an indent is REJECTED or CANCELLED, and the reason SHALL remain visible on the indent record.

#### Scenario: Rejection without reason is blocked
- **WHEN** the project manager attempts to reject a PENDING indent without selecting a reason code
- **THEN** the system SHALL block the rejection and keep the indent PENDING.

#### Scenario: Cancelled draft carries reason
- **WHEN** an engineer cancels a DRAFT indent with a reason code
- **THEN** the indent becomes CANCELLED with the reason recorded and visible.

### Requirement: Schedule-driven needed-by date and activity reference
Each indent line SHALL carry a needed-by date and an activity reference (e.g. Block B slab), and the pending approval queue SHALL sort by needed-by date ascending so the most schedule-critical indents surface first.

#### Scenario: Pending queue ordering
- **WHEN** the project manager opens the pending queue with multiple PENDING indents
- **THEN** indents SHALL be ordered by earliest line needed-by date first.

### Requirement: Overdue pending nudge without auto-approval
The system SHALL surface a nudge on PENDING indents whose needed-by date is within the risk window or past, and the system SHALL NOT auto-approve any indent regardless of overdue status.

#### Scenario: Slab-risk nudge appears, still pending
- **WHEN** a PENDING indent's needed-by date falls within the risk window
- **THEN** the indent SHALL display a risk nudge and remain PENDING until a human approves or rejects it.
