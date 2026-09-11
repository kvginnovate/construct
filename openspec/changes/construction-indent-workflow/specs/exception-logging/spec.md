## Purpose

Quarantines paperwork-after-fact indent entry in a flagged admin-only path so emergency site practice stays honest without polluting normal flow metrics.

## ADDED Requirements

### Requirement: Retro-entry restricted to admin screen
Retroactive indent entry SHALL be available only through a dedicated admin screen, never through the normal site-engineer indent flow, and every retro entry SHALL require a late-reason before it can be saved.

#### Scenario: Normal flow has no retro path
- **WHEN** a site engineer creates an indent in the normal flow
- **THEN** there SHALL be no option to mark it retroactive or backdate it into the exception path.

#### Scenario: Admin retro-entry without reason is blocked
- **WHEN** an admin attempts to save a retroactive indent without a late-reason
- **THEN** the system SHALL block the save and keep the entry unsaved.

### Requirement: Persistent visible retro flag
Every retroactive indent SHALL carry a persistent retro flag visible wherever the indent appears (queues, records, fulfilment screens), distinguishing it from normal-flow indents for its lifetime.

#### Scenario: Retro flag follows the indent
- **WHEN** a retroactive indent is approved and fulfilled
- **THEN** its detail, queue rows, and fulfilment legs SHALL all display the retro flag.

### Requirement: Retro exclusion from lead-time reports
Lead-time and approval-latency reports SHALL exclude retroactive indents by default, with an explicit opt-in required to include them.

#### Scenario: Default lead-time report excludes retro entries
- **WHEN** a user opens the indent lead-time report with default filters
- **THEN** retroactive indents SHALL be excluded from the computed statistics.

#### Scenario: Explicit opt-in includes retro entries
- **WHEN** a user enables the include-retroactive option on the report
- **THEN** retroactive indents SHALL appear in the statistics, marked as retroactive.

### Requirement: Retro frequency signal per engineer
The system SHALL maintain a per-engineer count of retroactive indents over a rolling window and surface it on the admin screen as a quiet operational signal, without blocking normal indent submission.

#### Scenario: Frequent retro use is visible to admins
- **WHEN** an engineer accumulates multiple retroactive indents within the rolling window
- **THEN** the admin screen SHALL display the elevated count next to that engineer while their normal submission rights remain unchanged.
