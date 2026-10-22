# PHM-001 — Define repayment and maturity after a payment holiday

Status: Draft  
Owner: Andrew Millard  
Last updated: 22 October 2026  
Product acceptance: Not accepted  
Related behaviour: BHV-008 Payment Holiday (existing expert backlog identifier; not yet modelled in behaviours.yaml)

## Purpose and scope

Outcome: Define how a payment holiday changes expected collections, interest and final repayment, without confusing projections with contractual obligations.

Confirmed task: Prepare requirements for the recommended capability, using the Loan Domain Expert workflow (D-001).

Confirmed first slice (D-002): One future holiday on a fixed-rate equal-instalments (ANNUITY) loan. Interest accrues separately (D-003). Repayments increase afterwards and the term stays unchanged (D-004). Holiday interest is spread across remaining post-holiday repayments; total repayments are equal, subject to a final rounding adjustment (D-006). Approval creates a new contractual schedule version and preserves the original for history (D-007). Operational DPD excludes holiday days and resumes afterwards for remaining overdue obligations; no separate total-days-overdue measure is added (D-008). Late-payment penalties pause during the holiday; ordinary interest continues separately (D-009). Existing penalties remain outstanding; post-holiday penalties resume only for remaining overdue amounts, without retrospective holiday charges (D-010). The new version becomes effective at holiday commencement; the existing schedule remains operative before then (D-011). Cancellation before commencement retains the future version as cancelled and keeps the existing schedule operative (D-012). Detailed eligibility and API/operator outputs remain to confirm.

Confirmed exclusion (D-013): Changes and cancellations after holiday commencement are deferred from the initial release. This does not authorise ad hoc corrections or deleting posted history. The unsupported-request response and exact commencement boundary remain to specify.

Future consideration (D-005): Term extension and final residual-payment policies may be added later. This does not require implementing configuration or alternative strategies now.

Remaining proposed exclusions — awaiting confirmation: Concurrent rate changes, overlapping holidays, backdated changes, interest-only/bullet loans, fee/penalty policy changes and full collections automation. Existing behaviour for excluded cases still needs regression protection; exclusion does not authorise breaking it.

Actor, authority, eligibility and approval: Open (Q-001/Q-006). Existing scheduled-event and arrangement paths are evidence, not an agreed new workflow.

## Current behaviour and evidence

| Area | Observed or documented position | Target |
| --- | --- | --- |
| Holiday projection | Forecast rows have zero total/principal due; interest can increase projected closing principal. Tests cover both accrue_interest values (E-003/E-004). | D-003: accrue interest separately; do not capitalise it into principal. Collection details remain Q-002. |
| After holiday | Annuity forecast retains reference instalment amounts and iterates existing future dates. Positive residual is returned separately (E-003). | D-004: increase post-holiday repayments within the unchanged term. Calculation details remain Q-003. |
| Final repayment | Payment-schedule spec says the final annuity row clears the balance. Code can leave residual; prior ticket resolved this through explicit residual reporting (E-005/E-006). | D-004 rules out selecting a final residual-payment policy for this release. Reconcile code/spec with repayment increase; rounding and last-date holiday treatment remain open. |
| Contract versus projection | Existing documents separate L1 contractual schedule, L2 actual position and L3 payment schedule. Holiday events do not themselves create a new version (E-001/E-005). | D-007: approval creates a new contractual schedule version, preserving the original. D-011 makes the new version effective at holiday commencement, with the existing version operative beforehand. D-012 fixes pre-start cancellation. Reconcile existing documents; exact boundary processing remains Q-004. |
| Servicing effects | Arrangement lifecycle exists, but source documents still defer scheduled-event transition rules (E-007). | Q-005: establish collection, arrears and actual accrual effects. |

## Required behaviour

The following obligations record confirmed product decisions. The complete capability remains Draft because detailed calculation, version lifecycle and operational effects are unresolved.

| ID | Requirement | Decision |
| --- | --- | --- |
| FR-001 | The system must support one future payment holiday for the initial fixed-rate annuity scope. | D-002; eligibility details Q-006 |
| FR-002 | The system must continue interest accrual during the payment holiday. | D-003 |
| FR-003 | The system must retain holiday-accrued interest separately from principal. | D-003 |
| FR-004 | The system must increase repayments after the holiday to recover deferred amounts within the existing term. | D-004; calculation details Q-002/Q-003 |
| FR-005 | The system must preserve the loan's maturity date when applying this initial holiday treatment. | D-004 |
| FR-006 | The system must spread holiday-accrued interest across the remaining post-holiday repayments. | D-006 |
| FR-007 | The system must calculate equal total post-holiday repayments, subject to a final rounding adjustment. | D-006 |
| FR-008 | The system must create a new contractual schedule version when the payment holiday is approved. | D-007 |
| FR-009 | The system must preserve the original contractual schedule version for historical reference. | D-007 |
| FR-010 | The system must exclude approved payment-holiday days from operational DPD ageing. | D-008 |
| FR-011 | The system must resume operational DPD ageing after the holiday for obligations that remain overdue. | D-008 |
| FR-012 | The system must recalculate operational DPD when payments clear the oldest overdue obligation. | D-008 |
| FR-013 | The system must prevent holiday-deferred instalments from adding operational DPD during the approved holiday. | D-008 |
| FR-014 | The system must suspend late-payment penalty accrual and assessment for the approved holiday period. | D-009 |
| FR-015 | The system must retain outstanding penalties already charged when the holiday begins. | D-010 |
| FR-016 | The system must resume post-holiday late-payment penalties only where overdue amounts remain. | D-010 |
| FR-017 | The system must make the approved holiday schedule version effective when the holiday starts. | D-011 |
| FR-018 | The system must use the existing contractual schedule until the holiday starts. | D-011 |
| FR-019 | The system must retain a pre-start-cancelled holiday schedule version with cancelled status for audit history. | D-012 |
| FR-020 | The system must keep the existing contractual schedule operative after pre-start holiday cancellation. | D-012 |
| FR-021 | The system must prevent a cancelled holiday schedule version from becoming operative. | D-012 |

| ID | Business rule | Decision |
| --- | --- | --- |
| BR-001 | Holiday interest must not be capitalised into principal under the initial treatment. | D-003 |
| BR-002 | Term extension and a final residual-payment strategy are outside the initial treatment. | D-004/D-005 |
| BR-003 | The spreading calculation must retain holiday-accrued interest separately from principal. | D-003/D-006 |
| BR-004 | Creating the holiday schedule version must not overwrite the original contractual schedule. | D-007 |
| BR-005 | This capability uses operational DPD only; it must not introduce a separate total-calendar-days-overdue measure. | D-008 |
| BR-006 | The late-payment penalty pause must not stop ordinary interest accrual under FR-002. | D-003/D-009 |
| BR-007 | The system must not charge retrospective late-payment penalties for approved holiday days. | D-010 |
| BR-008 | Approval-time creation must not make the future holiday schedule operative before holiday commencement. | D-011 |

Spreading is confirmed; the allocation order within each repayment, exact calculation and compound-interest policy remain unresolved. Do not infer that equal total repayments require equal deferred-interest components.

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

As remaining decisions are confirmed, promote only unresolved candidate details into additional atomic FR/BR statements, linked to decision IDs. FR-001–FR-021 and BR-001–BR-008 already record confirmed policy. Do not relabel candidates as confirmed merely because a draft is committed.

## Business rules and dependencies

Existing constraints to preserve or explicitly reconcile: append-only history; Decimal calculations and ADR-001 rounding/day ordering; schedule layer vocabulary; current event window semantics (inclusive due dates). These are platform evidence, not newly approved product defaults.

Material choices still open: detailed eligibility and scope boundaries; allocation of separately accrued interest within each repayment; exact repayment calculation and rounding precision; configuration owner/default; schedule-version cancellation and exact boundary handling; arrears handling and exceptions. See [decisions.md](decisions.md).

## Exceptions to resolve

| Condition | Required decision |
| --- | --- |
| Holiday includes final contractual due date; no scheduled rows remain afterwards | Q-003/Q-006: decide eligibility or rejection when no post-holiday dates remain; do not assume extension or a residual-payment policy |
| Recalculated repayment cannot satisfy the unchanged maturity or exceeds an agreed limit | Q-003/Q-006: agree feasibility limits and failure response without extending the term |
| Existing arrears when holiday begins | D-008 fixes DPD pause/resume. D-009 pauses new late-payment penalties. D-010 preserves existing penalties and restricts post-holiday resumption. Q-005/Q-006 still govern eligibility, monetary arrears/collection relief and exact boundaries. |
| Inverted/overlapping window or a date that is not a due date | Q-006: preserve or change documented validation explicitly |
| Cancellation before commencement | D-012: retain the cancelled future version and keep the existing schedule operative. Q-007: repeated-request response remains open. |
| Duplicate submission | Q-007: replay/conflict response remains open |
| Change or cancellation after commencement | D-013: outside initial supported scope. Q-007: exact unsupported-request response remains open. |
| Concurrent modification | Q-001/Q-007: scope and conflict behaviour remain open |

## Acceptance examples — decision probes, not approved tests

Use one fixed source schedule, business date, product snapshot and opening balance when numerical cases are agreed. Amounts below are exploratory fixture inputs, not independently calculated expectations.

| ID | Starting state / action | Expected result | Blocks |
| --- | --- | --- | --- |
| EX-001 | Active ANNUITY loan, 12 monthly rows remain, projected opening principal 10,000.00, annual rate 12%; holiday covers next two due dates. | Confirmed: interest accrues separately; principal does not increase from holiday interest; subsequent repayments increase and maturity stays unchanged. Holiday interest is spread across equal total remaining repayments, with a final rounding adjustment. Exact amounts and component allocation remain unresolved. | Q-001–Q-005 |
| EX-002 | Same fixture with interest disabled in holiday request. | D-003 requires separate accrual for this initial treatment. Whether an incompatible request is rejected or the field is unavailable remains Q-006; it must not silently select a different policy. | Q-002/Q-005 |
| EX-003 | Holiday covers the final contractual due date. | No term extension or residual-payment strategy is selected. Decide whether this boundary is ineligible when no recovery dates remain; exact response and modification record remain open. | Q-003/Q-004 |
| EX-004 | Confirmed future holiday is cancelled before it starts. | Retain the future version as cancelled for audit history; keep the existing schedule operative. Do not activate the cancelled version. Repeat-cancellation response remains open. | Q-007 repeated-request details |
| EX-005 | Account already has overdue interest when holiday is requested. | Operational DPD pauses during an approved holiday and resumes afterwards for remaining overdue obligations. Late-payment penalties pause during the holiday. Existing penalties remain outstanding and penalties resume only for remaining overdue amounts, without holiday catch-up. Eligibility and monetary arrears/collection treatment remain unresolved. | Q-005/Q-006 remaining details |
| EX-006 | Same confirmed event history is replayed. | Existing constraint INV-004 expects reproducibility; holiday-specific numerical oracle depends on confirmed rules. | Q-002–Q-005 |
| EX-007 | Ordinary annuity account has no holiday. | Compare pre/post-change outputs at a fixed baseline; no new policy should leak into this control case. Specific regression oracle to capture during implementation. | Confirmed scope and baseline |
| EX-008 | Holiday interest is 120.00 and ten post-holiday repayments remain. | Recover 120.00 across those repayments, separately from principal. Total repayments are equal except for final rounding. Do not assume each deferred-interest component is 12.00 until the allocation rule is agreed. | Q-002/Q-003 exact calculation and allocation |
| EX-009 | A future holiday is approved for an account with an existing contractual schedule. | Create a new contractual version reflecting agreed holiday repayment treatment; retain the original unchanged for history. New version is effective at holiday commencement; existing version remains operative until then. Pre-start cancellation retains a cancelled future version and keeps the existing schedule operative (D-012). Post-start changes/cancellations are deferred under D-013; unsupported-request details remain open. | Q-004 lifecycle details |

| EX-010 | Operational DPD is 15 at holiday entry. Thirty holiday days elapse; the same oldest obligation remains unpaid. | Operational DPD remains 15 throughout the holiday. No separate total-calendar-days-overdue measure is introduced. | Q-006 exact holiday boundary/day ordering |
| EX-011 | EX-010 holiday has ended. Five non-holiday ageing days elapse and the same obligation remains unpaid. | Operational DPD is 20. | Q-006 boundary/day ordering |
| EX-012 | All overdue obligations are cleared during the holiday; no other obligation is overdue at exit. | Operational DPD is zero; the entry count must not be restored at exit. | Payment allocation details remain Q-002/Q-005 |

| EX-013 | A loan with overdue obligations enters an approved holiday; ordinary interest remains applicable. | No late-payment penalties accrue or are assessed for holiday days. Ordinary interest continues accruing separately. Existing posted penalties remain outstanding. Resume penalties afterwards only for remaining overdue amounts; never charge for holiday days retrospectively. | Q-005/Q-006 remaining penalty and boundary details |

| EX-014 | Outstanding penalties are 50.00 at holiday entry; no payment, waiver or correction occurs during the holiday. Overdue amounts remain at exit. | Existing penalties remain 50.00 through the holiday. Post-holiday penalties may resume under applicable rules, but exclude all approved holiday days. | Q-006 exact boundaries; applicable rate/base |
| EX-015 | All overdue amounts are cleared before holiday exit; no new amount becomes overdue afterwards. | Do not resume late-payment penalties merely because the holiday ended. | Q-002/Q-005 allocation and arrears calculation |

| EX-016 | Holiday is approved on business date 1 June and starts on 1 July. | Create the future-effective version on 1 June. Use the existing contractual version for dates before 1 July, then the new version from 1 July. Preserve the original unchanged. | Q-004/Q-006 exact activation-day processing order |

| EX-017 | A future holiday version is cancelled before commencement; the business date then reaches its former start date. | The cancelled version remains available for audit history and must not activate. The existing contractual schedule remains operative. | Q-004/Q-006 exact business-date ordering |

| EX-018 | A request changes or cancels a holiday after commencement. | Outside the supported initial-release scope (D-013). No post-start correction workflow is specified. Agree exact rejection/response and boundary handling before engineering readiness. | Q-007 unsupported-request response; Q-006 boundaries |

Review status: Designed as discussion cases; no application tests executed. DPD examples reflect D-008; financial repayment amounts remain uncalculated and unapproved.

## Open decisions and readiness

Core scope, separate accrual and repayment-increase policy are confirmed (D-002–D-005). Material details within Q-001–Q-007 remain open. D-006 confirms spreading and equal total repayments with final rounding. D-007 confirms contractual version creation on approval and preservation of the original. D-008 confirms operational DPD pause/resume without a separate total-days measure. D-009 confirms the late-payment penalty pause. D-010 confirms existing penalty retention and prospective resumption without holiday catch-up. D-011 fixes the new version's effective date at holiday commencement. D-012 confirms pre-start cancellation and audit retention. D-013 defers post-start changes/cancellations. Next resolve monetary arrears/collection treatment, calculation details and remaining exception responses/boundaries. Not ready for engineering.
Implementation linkage: [implementation.md](implementation.md). Evidence and decision authority: [decisions.md](decisions.md).
