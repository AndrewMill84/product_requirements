# PHM-001 — Define repayment and maturity after a payment holiday

Status: Draft  
Owner: Andrew Millard  
Last updated: 22 October 2026  
Product acceptance: Not accepted  
Related behaviour: BHV-008 Payment Holiday (existing expert backlog identifier; not yet modelled in behaviours.yaml)

## Purpose and scope

Outcome: Define how a payment holiday changes expected collections, interest and final repayment, without confusing projections with contractual obligations.

Confirmed task: Prepare requirements for the recommended capability, using the Loan Domain Expert workflow (D-001).

Confirmed first slice (D-002): One future holiday on a fixed-rate equal-instalments (ANNUITY) loan. Interest accrues separately (D-003). Repayments increase afterwards and the term stays unchanged (D-004). Detailed eligibility and API/operator outputs remain to confirm.

Future consideration (D-005): Term extension and final residual-payment policies may be added later. This does not require implementing configuration or alternative strategies now.

Remaining proposed exclusions — awaiting confirmation: Concurrent rate changes, overlapping holidays, backdated changes, interest-only/bullet loans, fee/penalty policy changes and full collections automation. Existing behaviour for excluded cases still needs regression protection; exclusion does not authorise breaking it.

Actor, authority, eligibility and approval: Open (Q-001/Q-006). Existing scheduled-event and arrangement paths are evidence, not an agreed new workflow.

## Current behaviour and evidence

| Area | Observed or documented position | Target |
| --- | --- | --- |
| Holiday projection | Forecast rows have zero total/principal due; interest can increase projected closing principal. Tests cover both accrue_interest values (E-003/E-004). | D-003: accrue interest separately; do not capitalise it into principal. Collection details remain Q-002. |
| After holiday | Annuity forecast retains reference instalment amounts and iterates existing future dates. Positive residual is returned separately (E-003). | D-004: increase post-holiday repayments within the unchanged term. Calculation details remain Q-003. |
| Final repayment | Payment-schedule spec says the final annuity row clears the balance. Code can leave residual; prior ticket resolved this through explicit residual reporting (E-005/E-006). | D-004 rules out selecting a final residual-payment policy for this release. Reconcile code/spec with repayment increase; rounding and last-date holiday treatment remain open. |
| Contract versus projection | Existing documents separate L1 contractual schedule, L2 actual position and L3 payment schedule. Holiday events do not themselves create a new version (E-001/E-005). | Q-004: decide when a changed term/amount needs a modification/version. |
| Servicing effects | Arrangement lifecycle exists, but source documents still defer scheduled-event transition rules (E-007). | Q-005: establish collection, arrears and actual accrual effects. |

## Required behaviour

The following obligations record confirmed product decisions. The complete capability remains Draft because collection mechanics, contractual treatment and operational effects are unresolved.

| ID | Requirement | Decision |
| --- | --- | --- |
| FR-001 | The system must support one future payment holiday for the initial fixed-rate annuity scope. | D-002; eligibility details Q-006 |
| FR-002 | The system must continue interest accrual during the payment holiday. | D-003 |
| FR-003 | The system must retain holiday-accrued interest separately from principal. | D-003 |
| FR-004 | The system must increase repayments after the holiday to recover deferred amounts within the existing term. | D-004; calculation details Q-002/Q-003 |
| FR-005 | The system must preserve the loan's maturity date when applying this initial holiday treatment. | D-004 |

| ID | Business rule | Decision |
| --- | --- | --- |
| BR-001 | Holiday interest must not be capitalised into principal under the initial treatment. | D-003 |
| BR-002 | Term extension and a final residual-payment strategy are outside the initial treatment. | D-004/D-005 |

Separate accrual does not yet specify the interest collection order or whether a separately assessed charge is permitted. Do not infer a collection formula or compound-interest policy.

The following remaining candidates are recommendations for discussion; confirmed FR/BR entries above take precedence. They are deliberately non-normative and are not an approved implementation contract.

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

As remaining decisions are confirmed, promote only unresolved candidate details into additional atomic FR/BR statements, linked to decision IDs. FR-001–FR-005 and BR-001/002 already record confirmed policy. Do not relabel candidates as confirmed merely because a draft is committed.

## Business rules and dependencies

Existing constraints to preserve or explicitly reconcile: append-only history; Decimal calculations and ADR-001 rounding/day ordering; schedule layer vocabulary; current event window semantics (inclusive due dates). These are platform evidence, not newly approved product defaults.

Material choices still open: detailed eligibility and scope boundaries; recovery of separately accrued interest; repayment calculation and rounding; configuration owner/default; contractual amendment; arrears handling and exceptions. See [decisions.md](decisions.md).

## Exceptions to resolve

| Condition | Required decision |
| --- | --- |
| Holiday includes final contractual due date; no scheduled rows remain afterwards | Q-003/Q-006: decide eligibility or rejection when no post-holiday dates remain; do not assume extension or a residual-payment policy |
| Recalculated repayment cannot satisfy the unchanged maturity or exceeds an agreed limit | Q-003/Q-006: agree feasibility limits and failure response without extending the term |
| Existing arrears when holiday begins | Q-005: eligibility, pre-existing debt and DPD treatment |
| Inverted/overlapping window or a date that is not a due date | Q-006: preserve or change documented validation explicitly |
| Duplicate submission or cancellation before commencement | Q-007: replay/conflict and cancellation response |
| Change after commencement or concurrent modification | Q-001/Q-007: supported scope, deferral or rejection |

## Acceptance examples — decision probes, not approved tests

Use one fixed source schedule, business date, product snapshot and opening balance when numerical cases are agreed. Amounts below are exploratory fixture inputs, not independently calculated expectations.

| ID | Starting state / action | Expected result | Blocks |
| --- | --- | --- | --- |
| EX-001 | Active ANNUITY loan, 12 monthly rows remain, projected opening principal 10,000.00, annual rate 12%; holiday covers next two due dates. | Confirmed: interest accrues separately; principal does not increase from holiday interest; subsequent repayments increase and maturity stays unchanged. Exact amounts and recovery profile remain unresolved. | Q-001–Q-005 |
| EX-002 | Same fixture with interest disabled in holiday request. | D-003 requires separate accrual for this initial treatment. Whether an incompatible request is rejected or the field is unavailable remains Q-006; it must not silently select a different policy. | Q-002/Q-005 |
| EX-003 | Holiday covers the final contractual due date. | No term extension or residual-payment strategy is selected. Decide whether this boundary is ineligible when no recovery dates remain; exact response and modification record remain open. | Q-003/Q-004 |
| EX-004 | Confirmed future holiday is cancelled before it starts. | Unresolved: resulting schedule, record and repeat-cancellation response. | Q-007 |
| EX-005 | Account already has overdue interest when holiday is requested. | Unresolved: eligibility and old versus new shortfalls, DPD and penalties. | Q-005/Q-006 |
| EX-006 | Same confirmed event history is replayed. | Existing constraint INV-004 expects reproducibility; holiday-specific numerical oracle depends on confirmed rules. | Q-002–Q-005 |
| EX-007 | Ordinary annuity account has no holiday. | Compare pre/post-change outputs at a fixed baseline; no new policy should leak into this control case. Specific regression oracle to capture during implementation. | Confirmed scope and baseline |

Review status: Designed as discussion cases; no application tests executed and no numerical outcomes approved.

## Open decisions and readiness

Core scope, separate accrual and repayment-increase policy are confirmed (D-002–D-005). Material details within Q-001–Q-007 remain open. Next resolve Q-002 interest recovery, Q-003 repayment calculation and Q-004 contractual treatment. Not ready for engineering.
Implementation linkage: [implementation.md](implementation.md). Evidence and decision authority: [decisions.md](decisions.md).
