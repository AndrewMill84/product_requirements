# Checkpoint — Payment-holiday maturity treatment

Last updated: 22 October 2026  
Status: Draft discovery; awaiting product decisions

## Agreed task
D-001 authorises expert-led requirements. D-002 confirms one future holiday on a fixed-rate annuity. D-003 confirms separate interest accrual. D-004 confirms increased repayments with unchanged term. D-005 records alternative exit policies as possible later work. D-006 confirms spreading holiday interest across equal total remaining repayments with a final rounding adjustment. D-007 confirms a new contractual schedule version on approval, preserving the original. D-008 confirms operational DPD pause/resume and excludes a separate total-days-overdue measure. D-009 confirms pausing late-payment penalties during the holiday while ordinary interest continues. D-010 preserves existing penalties and resumes them prospectively only for remaining overdue amounts, excluding holiday days. D-011 confirms creation on approval and effectiveness at holiday commencement, with the existing schedule operative beforehand. D-012 confirms pre-start cancellation retains a cancelled future version and keeps the existing schedule operative. D-013 defers changes and cancellations after holiday commencement from the initial release. D-014 confirms eligibility with existing arrears, retained separately and excluded from automatic holiday repayment spreading. D-015 confirms a six-month maximum holiday duration. D-016 confirms one to six consecutive monthly instalments with at least one later repayment by unchanged maturity. D-017 permits voluntary holiday payments under current applicable allocation rules; holiday-specific rules remain future scope. D-018 confirms voluntary-payment balance/forecast updates while preserving the approved contractual schedule unless explicitly rescheduled. D-019 confirms holiday coverage from first deferred due date through the day before repayments resume, inclusive, for DPD and penalty pauses. D-020 confirms no interest on unpaid holiday interest and ordinary interest on outstanding principal only. D-021 confirms existing applicable approval controls and excludes a separate holiday-specific workflow. Full specification acceptance remains pending.

## Progress
Read the requirements workflow and actual platform Loan Domain Expert skill/prompt. Inspected graph, ADRs, schedule specification, holiday code, test definitions and related tickets.
Created requirements.md, decisions.md and implementation.md. Added capability index entry.

## Findings
Current forecast can capitalise holiday interest and leave a separately reported terminal residual. The final-row statement in the schedule spec marked approved domain design conflicts with that code path. Explicit residual reporting does not select term extension, recast or a final lump sum.
Operational holiday effects must be investigated separately from L3 projection.

## Next discussion — ask these first
1. Q-002/Q-003: Verify current allocation and interest calculation before proposing the separate holiday-interest repayment mapping. Business-day date selection and same-day order need reconciliation with platform rules.
2. Q-002/Q-003: Verify current applicable payment-allocation rules and map separate holiday interest without inventing precedence. D-018 resolves the schedule effect: update balances/forecast without automatic contractual recalculation.
3. Q-005: Agree collection handling for pre-existing arrears during the holiday; eligibility and separate retention are confirmed.

Do not reopen D-002–D-021 without a conflict or new instruction. Resolve operational effects and exceptions afterwards. Updated FR-001–FR-035, BR-001–BR-017 and affected examples; DPD examples added from the confirmed policy; no repayment amounts calculated.

## Approval control evidence to verify
D-021 selects reuse, not a new control design. Inspect actual platform permissions and approval paths; do not assume the backlog maker-checker/auth work is complete or silently waive it.

## Evidence freshness and validation
Platform evidence: `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`. Requirements instructions read at `9b086b4493966bd37e8640d33d9f3dd058ba6d3a`.
Source/test-definition inspection only. No runtime, UI or application tests executed. Numerical acceptance outcomes are not yet approved.
Before implementation, recheck affected source against the current platform head.

## Delivery handover
Requirements baseline: Not accepted or pinned for implementation.
Delivery state: Blocked; see [implementation.md](implementation.md).
