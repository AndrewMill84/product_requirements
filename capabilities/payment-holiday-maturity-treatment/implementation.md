# Implementation handoff — PHM-001

Target: https://github.com/AndrewMill84/loan-admin-platform  
Delivery state: Blocked — final product decisions and acceptance pending  
Product acceptance: Not accepted  
Accepted requirements baseline SHA: Not pinned  
Platform evidence SHA: `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`

## Agreed policy and scope

Read the authoritative [scope and confirmed requirements](requirements.md#purpose-and-agreed-scope) and [decision record](decisions.md). Remaining product blockers are listed in [requirements.md](requirements.md#remaining-decisions), with analysis in [reconciliation-review.md](reconciliation-review.md). Do not select unresolved behaviour in code or infer acceptance.

## Technical delivery work after decisions

| Area | Needed work / evidence |
| --- | --- |
| Calculations and balances | Separate non-interest-bearing holiday-interest tracking; revised equal repayments on existing dates; reconcile daily-rounded postings with forecasts; no double recovery or capitalisation. |
| Version lifecycle | Create future-effective version on approval, retain old version, choose operative version by date, retain pre-start-cancelled history, and replay deterministically. The ScheduleVersion dataclass alone does not provide this. |
| Old debt and allocations | Preserve applicable waterfall and pool semantics; implement confirmed interest attribution and old-principal partition. Allocation/history identity must survive schedule versions. |
| DPD/penalties | Exclude holiday days; recalculate after old debt clears; suppress holiday penalties without catch-up; retain posted debt; consistent operational outputs. |
| Approval | Enforce a general authenticated actor/permission map. Current actor headers and placeholder approval endpoints are not access control. Do not invent bespoke holiday maker-checker. |
| Preview/concurrency | Bind approved figures to relevant account/schedule/holiday/policy/date inputs. Compare current state and write atomically; sequence alone misses business-date/policy changes. |
| Dates and API | Use existing product business-day conventions and derive D-019 pause from selected due rows. Idempotent retry, conflict and unsupported-operation contracts follow existing conventions. |
| Documentation/graph | Reconcile platform specification saying holidays do not create versions, and final-row clearing statement versus residual-reporting implementation. Map BHV-008 only after agreement; no global RULE IDs yet assigned. |

Use [review evidence E-010–E-019](reconciliation-review.md) for immutable source links and implementation anchors. These are technical gaps, not newly open financial-policy choices.

## Coverage to establish

| Scoped requirements | Examples | Required evidence |
| --- | --- | --- |
| FR-001–FR-007/024–FR-026/032–FR-034; BR-001–BR-003/010/011/014–BR-016 | EX-001–EX-003/008/020–EX-023/026/027 | Independently reviewed money/date fixtures, clean and old-arrears cases, actual accrual versus forecast and final clearing. |
| FR-008/009/017–FR-021/031; BR-004/008/013 | EX-004/009/016–EX-018/025/033 | Future approval/activation/cancellation/version history and unchanged contract after receipts; persistence/replay. |
| FR-010–FR-016/022/023; BR-005–BR-007/009 | EX-005/010–EX-015/019/031/034 | DPD pause/resume, arrears clearance, no duplicate old debt, penalty pause with no retrospective charge. |
| FR-027–FR-030; BR-012 | EX-024/025 | Allocation at actual effective dates, ordinary/holiday interest attribution, receipts/reversals/replay without changing contract. |
| FR-035–FR-041; BR-017 | EX-028–EX-030/032 | Allowed/rejected actors, displayed preview, payment/date/policy stale checks, atomic approval and duplicate requests. |
| Cross-cutting | EX-006/007 | Replay parity, unchanged no-holiday controls, financial invariants and regression suite. |

Existing tests were read, not run. Do not claim delivered or verified coverage.

## Delivery references and acceptance

Platform spec/tickets, code branch/PR and implementation commit: Not created. No application changes authorised by this handoff.
After final acceptance, commit the accepted records and pin their full SHA here in a subsequent commit. Create the platform execution brief and numbered tickets under .scratch/<feature-slug>/, following current platform AGENTS.md. Record actual commands/results, environment, PR/commit and limitations for each scoped requirement. Merge/deployment need their own authorisation.
