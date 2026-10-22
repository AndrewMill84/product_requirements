# PHM-001 — Repayment and maturity after a payment holiday

Status: Draft  
Owner: Andrew Millard  
Last reviewed: 22 October 2026  
Product acceptance: Not accepted  
Related behaviour: BHV-008, expert backlog identifier; not a locked graph entry.

## Purpose and agreed scope

Define the holiday's contractual repayments, separate interest, operational treatment and approval preview.

| Area | Confirmed treatment |
| --- | --- |
| Product/window | One future holiday on a fixed-rate monthly annuity; one to six consecutive instalments, leaving at least one later repayment by unchanged maturity (D-002/D-015/D-016/D-019). |
| Interest/repayment | Separate, non-interest-bearing holiday interest; equal increased total repayments after the holiday, final rounding adjustment, unchanged maturity (D-003–D-006/D-020). |
| Contract/history | New version created on approval, effective at holiday start. Preserve original; pre-start cancellation retains cancelled history. No automatic contract rewrite after voluntary payment (D-007/D-011–D-013/D-018). |
| Servicing | Pause operational DPD and new late penalties over holiday days; preserve old arrears/penalties. Resume only for remaining overdue obligations. Existing arrears alone do not disqualify a borrower (D-008–D-010/D-014). |
| Payments/approval | Voluntary receipts use current applicable allocation; existing general approval controls apply. Show pre-approval preview and reject stale figures (D-017/D-021–D-023). |

Term extension/final residual strategies are future options (D-005). Post-start holiday changes/cancellations are deferred (D-013). Other products are outside D-002's initial scope. Overlap/concurrent-modification handling and remaining eligibility require R-003.

Confirmed policy is separate from final specification acceptance. The four grouped decisions in [reconciliation-review.md](reconciliation-review.md) are the remaining product blockers; implementation gaps do not reopen settled choices.

## Current versus target

| Current evidence | Required change / remaining boundary |
| --- | --- |
| L3 holiday code capitalises projected interest and can leave terminal residual (E-003/E-011). Actual COB accrues on principal into a common interest receivable (E-010). | Retain principal-only actual accrual, track holiday interest separately and reconcile projection with postings. |
| Existing future rows/reference payments are retained; holiday event does not create a contractual version (E-011/E-012). | Implement D-004/D-007/D-011/D-012. Approval/start/exit estimate differences remain R-002. |
| DPD and COB penalties have no holiday-day input (E-013/E-014). | Apply confirmed pauses and consistent outputs; separate old arrears from newly deferred obligations. |
| Existing allocator has one interest pool and allocation identity mainly by instalment number (E-017). | Preserve component waterfall; decide holiday/ordinary interest attribution and avoid duplicate old principal (R-001/R-003). |
| API approval controls/preview safeguards are incomplete (E-015/E-016). | Establish general actor permission mapping, atomic freshness check and version lifecycle. |

Evidence uses platform commit `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`. See [decisions.md](decisions.md) and the review.

## Required behaviour and business rules

These statements record confirmed decisions. They are not a complete accepted implementation baseline.

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
| FR-022 | The system must allow otherwise eligible borrowers with existing arrears to receive a payment holiday. | D-014 |
| FR-023 | The system must retain pre-existing arrears separately from amounts deferred by the payment holiday. | D-014 |
| FR-024 | The system must enforce a maximum payment-holiday duration of six months. | D-015/D-016 |
| FR-025 | The system must limit the initial monthly holiday to one to six consecutive scheduled instalments. | D-016 |
| FR-026 | The system must require at least one scheduled repayment after the holiday by the unchanged maturity date. | D-016 |
| FR-027 | The system must allow voluntary payments during an approved payment holiday. | D-017 |
| FR-028 | The system must allocate voluntary holiday payments using the current payment-allocation rules applicable to the loan. | D-017 |
| FR-029 | The system must update affected balances after applying a voluntary holiday payment. | D-018 |
| FR-030 | The system must update the forward forecast to reflect the voluntary payment. | D-018 |
| FR-031 | The system must preserve the approved contractual schedule when applying a voluntary holiday payment. | D-018 |
| FR-032 | The system must start the holiday on the first deferred instalment's due date. | D-019 |
| FR-033 | The system must end the holiday on the day before repayments resume. | D-019 |
| FR-034 | The system must calculate ordinary interest on outstanding principal only under the initial holiday treatment. | D-020 |
| FR-035 | The system must apply the platform's existing applicable approval permissions and controls to holiday requests. | D-021 |
| FR-036 | The system must display holiday start and end dates to the operator before approval. | D-022 |
| FR-037 | The system must display separately accrued holiday interest in the pre-approval preview. | D-022; future amounts are projected |
| FR-038 | The system must display revised post-holiday repayments in the pre-approval preview. | D-022 |
| FR-039 | The system must display the unchanged maturity date in the pre-approval preview. | D-022 |
| FR-040 | The system must require a refreshed preview after a payment or other relevant state change before approval. | D-023 |
| FR-041 | The system must prevent approval based on a stale preview. | D-023 |

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
| BR-009 | The holiday repayment recalculation must not automatically spread pre-existing arrears into revised repayments. | D-014 |
| BR-010 | A holiday longer than six months is outside the permitted initial-release limit. | D-015/D-016 |
| BR-011 | A holiday must not consume all remaining scheduled repayment dates. | D-016 |
| BR-012 | The initial holiday capability must not introduce a holiday-specific allocation waterfall. | D-017 |
| BR-013 | A voluntary payment must not trigger contractual schedule recalculation without an explicit reschedule. | D-018 |
| BR-014 | DPD and penalty pauses include the holiday start and end dates, excluding the resumed repayment date. | D-019 |
| BR-015 | Unpaid holiday-accrued interest must not earn interest, including after the holiday while it remains unpaid. | D-020 |
| BR-016 | Ordinary interest must exclude the separate holiday-interest balance from its calculation base. | D-020 |
| BR-017 | A separate holiday-specific approval workflow is outside the initial release. | D-021 |

## Remaining decisions and candidate disposition

| Question | Remaining product issue |
| --- | --- |
| Q-002/Q-003 — R-001 | Exact level-payment formula, holiday-interest component profile and partial/voluntary payment attribution. |
| Q-008 — R-002 | Reconcile approved projected figures with later actual balances/accrual without silently rewriting contractual rows. |
| Q-001/Q-005/Q-006 — R-003 | Zero deferred scheduled collections, collection of old debt, principal/interest partition and exceptional eligibility. |
| Q-006 — R-004 | Identify usable general approval authority because current holiday routes do not enforce a permission map. |

The earlier proposal IDs are retained for traceability, not competing requirements:

| Candidate | Disposition |
| --- | --- |
| P-001 | Superseded by FR-025/026/032/033/036; calendar integration is implementation work. |
| P-002 | Confirmed policy in FR-002/003/034 and BR-001/003/015/016; numerical method still R-001/R-002. |
| P-003 | Confirmed direction in FR-004–FR-007; exact amounts remain R-001/R-003. |
| P-004 | Superseded by FR-005/008/009/017/018/039; no revised maturity for initial treatment. |
| P-005 | Initial residual strategy excluded by BR-002; final clearing/reconciliation remains R-001/R-002. |
| P-006 | Confirmed FR-010–FR-016/022/023; old-debt/collection boundary still R-003. |
| P-007 | Confirmed pre-start cancellation FR-019–FR-021; repeat/conflict/error contract is implementation detail under Q-007. |
| P-008 | Existing append-only/replay constraints E-008 apply; version/replay integration requires implementation. |

## Exceptions and observable responses

Existing ACTIVE-account lifecycle, inclusive due-date windows, non-overlap and product date conventions are source constraints to preserve or explicitly change. R-003 confirms their application to the final release boundary.

Requests exceeding six instalments or leaving no later repayment are ineligible (FR-024–FR-026). Post-start changes/cancellations are unsupported (D-013). Invalid, conflicting or unsupported requests must not create a partial approved version, activate a cancelled version or mutate financial history. This is an integrity interpretation for review, not a new agreed correction workflow. Use existing problem-detail/idempotency conventions; exact endpoint/error-code design belongs to implementation.

Do not treat omission of a final repayment as permission to extend maturity. Do not label old debt twice as both separately overdue principal and newly amortised principal.

## Acceptance examples

Financial fixtures need fixed business/as-of dates, actual adjusted due dates, product snapshot, principal/interest/arrears buckets and posting-policy version. No numerical repayment oracle has yet been approved.

| ID | Starting state / action | Expected outcome or unresolved part |
| --- | --- | --- |
| EX-001 | Principal 10,000.00; annual rate 12%; 12 rows remain; next two rows deferred. | Separate holiday interest; increased equal total subsequent repayments; unchanged maturity. Exact components/amounts: R-001/R-002. |
| EX-002 | Same case, but request disables holiday interest. | Incompatible with D-003; no silent alternate interest treatment. Validation/API design required. |
| EX-003 | Holiday consumes final contractual due date; no later row remains. | Ineligible under D-016. No automatic extension. |
| EX-004 | Cancel approved holiday before commencement. | Retain cancelled future version; existing schedule remains operative. |
| EX-005 | Old overdue interest exists on otherwise eligible account. | Allow arrears; retain separately, pause DPD/penalties, resume for remaining overdue debt. Collection/interest recovery boundary: R-003. |
| EX-006 | Replay same event history. | INV-004 requires same balances, version selection, DPD and penalties. Exact money oracle follows R-001–R-003. |
| EX-007 | No holiday on a control account. | Preserve existing product behaviour; fixed regression baseline required. |
| EX-008 | Holiday interest H=120.00; ten repayments remain. | Recover H without interest-on-interest, in equal total repayments except final rounding. H/10 components are recommended, not yet selected (R-001). |
| EX-009 | Approve a future holiday. | Create new contractual version, preserve original, defer effectiveness to holiday start. |
| EX-010 | Entry DPD=15; 30 approved holiday days pass; old debt unpaid. | Operational DPD remains 15. Do not add a new total-days metric. |
| EX-011 | Five non-holiday ageing days follow EX-010; same old debt unpaid. | Operational DPD=20. |
| EX-012 | Payment clears all overdue debt during holiday; none overdue on exit. | DPD=0; do not restore entry count. |
| EX-013 | Late penalties would normally arise during holiday. | No holiday-period late penalties; ordinary interest continues separately. |
| EX-014 | Penalty balance=50.00 at entry; no payments/waivers/corrections. | Remains 50.00 through holiday; prospective resumption only if overdue debt remains. |
| EX-015 | Old debt cleared before exit; no new overdue amount. | Do not resume penalties merely because holiday ended. |
| EX-016 | Approval 1 June; holiday start 1 July. | Create version in June; old version operative before July; new version effective 1 July. Pre-start actual changes: R-002. |
| EX-017 | Cancel before start; former start date arrives. | Cancelled version never activates; audit history retained. |
| EX-018 | Change/cancel after commencement. | Unsupported initial scope. No invented correction path. |
| EX-019 | Old arrears=500.00; no settlement occurs. | Retain separately; no automatic spreading. Principal portion must not also be included in recast principal (R-003). |
| EX-020 | Six deferred instalments with at least one later repayment. | Meets count/remaining-row constraints; other agreed eligibility still applies. Use D-019 calendar coverage. |
| EX-021 | Seven deferred instalments requested. | Exceeds maximum; not approved. |
| EX-022 | Seven rows remain; first six deferred. | Meets count and remaining-row limits; unchanged maturity. |
| EX-023 | Six rows remain; all six deferred. | Ineligible because no later repayment remains. |
| EX-024 | Voluntary receipt during holiday; old debt and holiday interest exist. | Use current applicable waterfall, update debt/DPD; holiday-interest attribution still R-001. |
| EX-025 | Apply voluntary receipt; no explicit reschedule. | Update balances/L3; preserve contractual amounts/dates/version and holiday. |
| EX-026 | Deferred due dates 1 July/1 August; repayments resume 1 September. | Pause DPD/penalties 1 July–31 August inclusive: 62 days. Resume normal rules 1 September; principal-only interest continues. |
| EX-027 | Principal=10,000.00; unpaid holiday interest=120.00. | Ordinary interest base=10,000.00, never 10,120.00; daily rounding follows existing policy. |
| EX-028 | Two actors attempt approval. | Apply agreed general permissions; no assumed dual approval. Actor mapping remains R-004. |
| EX-029 | Generate pre-approval preview. | Dates, separate projected H, revised repayments and unchanged maturity shown. Projected versus posted amounts distinguished. |
| EX-030 | Payment follows preview but precedes approval. | Reject stale approval; require refreshed figures. Compare-and-write atomically (implementation). |
| EX-031 | Old overdue principal is already included in ledger principal. | Recast plus retained old claims must count it once, not twice. Exact partition/interest treatment: R-003. |
| EX-032 | Business date changes after preview without account-sequence change. | Detect relevant input changes, not only payment sequence; refresh before approval as D-023 requires. |
| EX-033 | Actual H differs from approved projected H after a voluntary payment. | Update L3, preserve approved contract (D-018); recovery of difference needs R-002. |
| EX-034 | Holiday-deferred row falls due; old arrears also exist. | Zero new scheduled collection/old-debt collection boundary must be confirmed in R-003; DPD/penalty pauses remain confirmed. |

Review only: existing tests were inspected, not executed. New examples are not claims of implemented coverage.

## Readiness and handoff

Four product groups R-001–R-004 block completeness; user acceptance of the final specification is pending. Once resolved, approve numerical fixtures, reconcile shared platform specifications/ADRs and obtain explicit acceptance. Then pin an immutable accepted baseline in [implementation.md](implementation.md). Technical work and evidence are summarised in [reconciliation-review.md](reconciliation-review.md).
