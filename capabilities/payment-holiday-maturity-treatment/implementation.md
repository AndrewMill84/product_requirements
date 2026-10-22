# Implementation handoff — PHM-001

Target: https://github.com/AndrewMill84/loan-admin-platform  
Delivery state: Blocked — product decisions pending  
Product acceptance of requirements: Not accepted  
Accepted by / date / scope: None  
Accepted requirements baseline commit SHA: Not pinned  
Platform evidence commit SHA: `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`

## Scope and blockers
Draft discussion scope only: [requirements.md](requirements.md).
D-001 confirms the task; D-002–D-019 confirm initial scope and treatment. FR-001–FR-033 and BR-001–BR-014 express those decisions. The full FR/BR set is incomplete and not accepted as an implementation baseline. Remaining candidates and numerical EX outcomes require decisions.
Blocking questions: Q-001–Q-007 in [decisions.md](decisions.md).
Do not implement a holiday policy from this draft.

Confirmed exclusion: Do not implement post-commencement holiday changes or cancellations in this release (D-013). Pre-start cancellation remains supported (D-012). Record and agree the unsupported-request contract; do not silently invent a correction path.

D-015 fixes the maximum duration at six months. Do not assume six months equals 180 days. D-016 fixes one to six consecutive monthly instalments and requires at least one later scheduled repayment by unchanged maturity. D-019 fixes the pause from first deferred due date through the day before repayments resume, inclusive. Business-day/date selection, same-day processing order and validation responses remain to establish.

D-017 allows voluntary payments during the holiday under the current allocation rules applicable to the loan. Investigate and reference those exact rules at a recorded platform commit. Do not infer a new waterfall, interest bucket priority, automatic reschedule or holiday cancellation from receipt of a payment. Holiday-specific allocation is future consideration only. D-018 requires balance/forecast updates with no automatic contractual schedule recalculation; an explicit reschedule is required to change contractual obligations. This does not bring the post-start holiday amendments deferred by D-013 into scope.

## Existing mappings and constraints
BHV-008 is the Payment Holiday identifier in the expert backlog, not a locked current graph entry.
Relevant existing invariants: INV-001/002/004/006/008/011; INV-005/009 apply to their defined contractual schedule scope. INV-003/007 matter if payment/cash handling is changed.
D-007 requires approval-triggered contractual schedule version creation while preserving the original. Existing documents describe holidays without a new version; implementation must reconcile that documented intent and investigate deferred versioning support. D-011 fixes effectiveness at holiday commencement, keeping the existing version operative before then. D-012 requires retaining the pre-start-cancelled version for audit, keeping the existing schedule operative and preventing future activation. D-013 defers post-start changes/cancellations from the initial release. Exact unsupported-request responses, repeated-request responses and processing order remain open.
No new RULE identifiers have been assigned. Preserve current graph and ADR authority; reconcile the schedule-spec final-row conflict explicitly.

D-008 requires operational DPD to exclude holiday days and resume for remaining overdue obligations; do not add a second calendar-days measure. Recalculate after arrears clearance. Verify persistence/replay and boundary handling. D-009 pauses holiday-period late-payment penalties while ordinary interest continues. D-010 preserves existing penalties and restricts resumption to remaining overdue amounts, without retrospective holiday penalties. D-014 allows otherwise eligible accounts with arrears; retain old arrears separately without automatically spreading them into revised repayments. Collection/allocation and exact boundaries remain open.

## Likely investigation areas after decisions
ForecastEngine; ScheduledEvent and arrangement transition; actual COB accrual and ledger treatment; L2 arrears/DPD; schedule versioning; API/UI maturity and repayment outputs.
These are investigation anchors, not an implementation design or approved file-change list.

## Implementation references
Platform local spec / tickets: Not created.
Platform branch / PR / code commit: Not created.
GitHub issue: None created.
When approved, create the platform execution brief and numbered tickets under .scratch/<feature-slug>/ using current platform instructions. Reference immutable accepted requirement/decision links instead of copying competing rules.

## Coverage and verification
| Proposed area | Discussion examples | Existing evidence | Gap |
| --- | --- | --- | --- |
| Holiday rows and interest | EX-001/002 | Existing holiday test definitions (E-004) | Actual accrual/ledger and agreed numerical expectations |
| Maturity and repayment | EX-001/003 | Residual reporting code/ticket (E-003/E-006) | Equal total repayments with final rounding selected; exact calculation, allocation and final amount unresolved |
| Cancellation/eligibility | EX-004/005/017 | Window semantics and existing lifecycle (E-007/E-009) | Accepted exceptions and end-to-end tests |
| Operational DPD | EX-010–EX-012 | D-008 product decision; no executed tests | Entry/exit boundaries, repayment during holiday and replay coverage |
| Penalty pause and resumption | EX-013–EX-015 | D-009/D-010 product decisions; no executed tests | Existing balances, no holiday catch-up, cleared arrears and exact boundaries |
| Version creation and effectiveness | EX-009/016 | D-007/D-011 product decisions; no executed tests | Creation versus effective-date queries, activation order, cancellation and replay |
| Existing arrears | EX-005/019 | D-014 product decision; no executed tests | Other eligibility, collection handling, allocation and no double counting of overdue principal |
| Duration and near-maturity eligibility | EX-003/020–EX-023 | D-015/D-016 product decisions; no executed tests | Count validation, month-end calendar dates, remaining repayment and error responses |
| Voluntary holiday payments | EX-024/025 | D-017/D-018 product decisions; no executed tests | Allocation/interest mapping, cleared arrears, updated forecast and unchanged contractual rows/version |
| Calendar-day holiday coverage | EX-026 | D-019 product decision; no executed tests | Inclusive start/end, resumed-date exclusion, variable month lengths and same-day order |
| Replay/regression | EX-006/007 | Existing invariants (E-008) | Fixed baseline fixtures and executed outcomes |

No application tests executed. No delivered acceptance.
After product approval: commit the accepted content, pin its full SHA here in a subsequent commit, recheck platform changes, and map each approved FR/BR to examples, tests and exact delivered commit.
