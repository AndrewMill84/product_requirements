# PHM-001 — Define repayment and maturity after a payment holiday

Status: Draft  
Owner: Andrew Millard  
Last updated: 22 October 2026  
Product acceptance: Not accepted  
Related behaviour: BHV-008 Payment Holiday (existing expert backlog identifier; not yet modelled in behaviours.yaml)

## Purpose and scope

Outcome: Define how a payment holiday changes expected collections, interest and final repayment, without confusing projections with contractual obligations.

Confirmed task: Prepare requirements for the recommended capability, using the Loan Domain Expert workflow (D-001).

Proposed first slice — awaiting Q-001: One future-dated holiday on an active, fixed-rate equal-instalments (ANNUITY) loan. Decide holiday interest and post-holiday repayment/maturity treatment. Cover API and operator-visible outputs.

Proposed exclusions — awaiting Q-001: Concurrent rate changes, overlapping holidays, backdated changes, interest-only/bullet loans, fee/penalty policy changes and full collections automation. Existing behaviour for excluded cases still needs regression protection; exclusion does not authorise breaking it.

Actor, authority, eligibility and approval: Open (Q-001/Q-006). Existing scheduled-event and arrangement paths are evidence, not an agreed new workflow.

## Current behaviour and evidence

| Area | Observed or documented position | Target |
| --- | --- | --- |
| Holiday projection | Forecast rows have zero total/principal due; interest can increase projected closing principal. Tests cover both accrue_interest values (E-003/E-004). | Q-002: agree interest economics, not just a forecast switch. |
| After holiday | Annuity forecast retains reference instalment amounts and iterates existing future dates. Positive residual is returned separately (E-003). | Q-003: agree how remaining debt is collected. |
| Final repayment | Payment-schedule spec says the final annuity row clears the balance. Code can leave residual; prior ticket resolved this through explicit residual reporting (E-005/E-006). | Reconcile intended policy before coding. Residual warning is not a selected collection mechanism. |
| Contract versus projection | Existing documents separate L1 contractual schedule, L2 actual position and L3 payment schedule. Holiday events do not themselves create a new version (E-001/E-005). | Q-004: decide when a changed term/amount needs a modification/version. |
| Servicing effects | Arrangement lifecycle exists, but source documents still defer scheduled-event transition rules (E-007). | Q-005: establish collection, arrears and actual accrual effects. |

## Required behaviour

No new financial behaviour is confirmed. There are no implementation-ready FR or BR entries. D-001 authorises analysis, not a choice of lending policy.

The following candidates are recommendations for discussion. They are deliberately non-normative and are not an approved implementation contract.

| Candidate ID | Proposed behaviour | Decisions required |
| --- | --- | --- |
| P-001 | Identify the affected collection dates and show the holiday's agreed treatment. | Q-001/Q-006 |
| P-002 | Calculate interest and deferred amounts using the selected treatment, effective dates and rounding rules. | Q-002/Q-006 |
| P-003 | Calculate subsequent instalment amounts, dates and final repayment using the selected exit treatment. | Q-003 |
| P-004 | Distinguish original contractual maturity, any revised contractual maturity and projected final collection date. | Q-003/Q-004 |
| P-005 | Explain any unpaid terminal amount and its agreed collection treatment. Do not imply zero debt from the last displayed date alone. | Q-003 |
| P-006 | Apply the agreed holiday treatment consistently to collection dues, L2 shortfall, DPD, penalties and actual interest postings. | Q-005 |
| P-007 | Apply cancellation before commencement and repeat requests without unintended financial effects. | Q-006/Q-007 |
| P-008 | Preserve historical records and reproduce agreed outcomes on replay. | Q-004/Q-005; existing platform constraints E-008 |

After Andrew decides, replace applicable candidates with atomic FR-001… and BR-001… statements, linked to decision IDs. Do not relabel candidates as confirmed merely because a draft is committed.

## Business rules and dependencies

Existing constraints to preserve or explicitly reconcile: append-only history; Decimal calculations and ADR-001 rounding/day ordering; schedule layer vocabulary; current event window semantics (inclusive due dates). These are platform evidence, not newly approved product defaults.

Material choices still open: scope, interest capitalisation versus separate interest balance, repayment/maturity treatment, configuration owner/default, contractual amendment, arrears handling and exceptions. See [decisions.md](decisions.md).

## Exceptions to resolve

| Condition | Required decision |
| --- | --- |
| Holiday includes final contractual due date; no scheduled rows remain afterwards | Q-003: supported extension/recast/final-payment outcome or rejection; no assumed default |
| Proposed extension cannot amortise at the retained payment, or exceeds an agreed limit | Q-003/Q-006: limit and failure response |
| Existing arrears when holiday begins | Q-005: eligibility, pre-existing debt and DPD treatment |
| Inverted/overlapping window or a date that is not a due date | Q-006: preserve or change documented validation explicitly |
| Duplicate submission or cancellation before commencement | Q-007: replay/conflict and cancellation response |
| Change after commencement or concurrent modification | Q-001/Q-007: supported scope, deferral or rejection |

## Acceptance examples — decision probes, not approved tests

Use one fixed source schedule, business date, product snapshot and opening balance when numerical cases are agreed. Amounts below are exploratory fixture inputs, not independently calculated expectations.

| ID | Starting state / action | Expected result | Blocks |
| --- | --- | --- | --- |
| EX-001 | Active ANNUITY loan, 12 monthly rows remain, projected opening principal 10,000.00, annual rate 12%; holiday covers next two due dates. | Unresolved: specify holiday interest, balance buckets, post-holiday payment and payoff date. | Q-001–Q-005 |
| EX-002 | Same fixture with interest disabled in holiday request. | Unresolved: determine whether interest truly stops, is waived, or only excluded from projection; reconcile actual COB. | Q-002/Q-005 |
| EX-003 | Holiday covers the final contractual due date. | Unresolved: exact final collection amount/date, residual treatment and modification record. | Q-003/Q-004 |
| EX-004 | Confirmed future holiday is cancelled before it starts. | Unresolved: resulting schedule, record and repeat-cancellation response. | Q-007 |
| EX-005 | Account already has overdue interest when holiday is requested. | Unresolved: eligibility and old versus new shortfalls, DPD and penalties. | Q-005/Q-006 |
| EX-006 | Same confirmed event history is replayed. | Existing constraint INV-004 expects reproducibility; holiday-specific numerical oracle depends on confirmed rules. | Q-002–Q-005 |
| EX-007 | Ordinary annuity account has no holiday. | Compare pre/post-change outputs at a fixed baseline; no new policy should leak into this control case. Specific regression oracle to capture during implementation. | Confirmed scope and baseline |

Review status: Designed as discussion cases; no application tests executed and no numerical outcomes approved.

## Open decisions and readiness

Q-001–Q-007 remain open. First discuss Q-001, Q-002 and Q-003. Not ready for engineering.
Implementation linkage: [implementation.md](implementation.md). Evidence and decision authority: [decisions.md](decisions.md).
