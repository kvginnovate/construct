## Purpose

Defines how approved indent lines are fulfilled from store stock, yard transfers, or purchase demand, with explicit human decisions on partial balances and site receipt close-out.

## ADDED Requirements

### Requirement: Per-line fulfilment split
Each APPROVED indent line SHALL be fulfilled through one or more fulfilment legs of type store issue, yard transfer, or purchase demand, with fulfilled quantity tracked per leg and never exceeding the line quantity across all legs.

#### Scenario: Split fulfilment across stock and purchase
- **WHEN** an APPROVED line for 200 bags is fulfilled with a 120-bag store issue plus an 80-unit purchase demand
- **THEN** the line SHALL show 120 fulfilled via issue, 80 pending via purchase, and zero unresolved balance hiding outside a leg.

#### Scenario: Over-fulfilment is blocked
- **WHEN** a fulfilment leg would push a line's total fulfilled quantity above its approved quantity
- **THEN** the system SHALL block the leg and leave recorded quantities unchanged.

### Requirement: Manual balance decision bucket with no auto-PO
Any unfulfilled balance on a PART-FULFILLED line SHALL sit in an explicit decision bucket until a human resolves it by raising a purchase demand, re-issuing from another store, or short-closing with a reason. The system SHALL NOT auto-create purchase orders from balances.

#### Scenario: Balance resolved by purchase demand
- **WHEN** a user converts a balance of 80 bags into a purchase demand
- **THEN** the balance SHALL leave the decision bucket and attach to the new purchase leg for tracking.

#### Scenario: No automatic purchase order exists
- **WHEN** a line becomes PART-FULFILLED with a remaining balance and no human action is taken
- **THEN** the system SHALL create no purchase order and the balance SHALL remain visible in the decision bucket.

### Requirement: Short-close with reason
Short-closing a balance SHALL require a reason, SHALL freeze the line at its fulfilled quantity, and SHALL record the short-closed quantity distinctly from fulfilled quantity.

#### Scenario: Short-close with reason
- **WHEN** a user short-closes a 50-bag balance with reason "managed with available stock"
- **THEN** the line SHALL close at its fulfilled quantity with 50 recorded as short-closed and the reason visible.

#### Scenario: Short-close without reason is blocked
- **WHEN** a user attempts to short-close a balance without a reason
- **THEN** the system SHALL block the action and keep the balance in the decision bucket.

### Requirement: Site receipt confirmation distinct from issue
Store issue SHALL NOT count as site receipt. A fulfilment leg of type store issue or transfer SHALL require site receipt confirmation recording received quantity and receiver identity before the leg counts as received.

#### Scenario: Issue awaits receipt
- **WHEN** 120 bags are issued from store but the site has not confirmed receipt
- **THEN** the leg SHALL show issued 120 and received 0, and the line SHALL NOT advance toward FULFILLED on issue alone.

#### Scenario: Receipt records shortage
- **WHEN** the site confirms receipt of 115 bags against 120 issued with receiver identity
- **THEN** the leg SHALL record received 115, the 5-unit gap SHALL remain visible, and the receiver identity SHALL be stored on the leg.

### Requirement: Vehicle and gate-pass record at issue
Each store-issue leg SHALL record vehicle and gate-pass details at the time of issue, and these details SHALL travel with the leg through to receipt confirmation.

#### Scenario: Issue carries gate-pass to receipt
- **WHEN** a storekeeper issues material with vehicle number and gate-pass recorded
- **THEN** the site receipt screen SHALL display the same vehicle and gate-pass details alongside the quantities to confirm.
