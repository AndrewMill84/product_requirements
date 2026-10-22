# Checkpoint — Payment-holiday maturity treatment

Last updated: 22 October 2026  
Status: Draft discovery; awaiting product decisions

## Agreed task
D-001 authorises expert-led requirements. D-002 confirms one future holiday on a fixed-rate annuity. D-003 confirms separate interest accrual. D-004 confirms increased repayments with unchanged term. D-005 records alternative exit policies as possible later work. D-006 confirms spreading holiday interest across equal total remaining repayments with a final rounding adjustment. Full specification acceptance remains pending.

## Progress
Read the requirements workflow and actual platform Loan Domain Expert skill/prompt. Inspected graph, ADRs, schedule specification, holiday code, test definitions and related tickets.
Created requirements.md, decisions.md and implementation.md. Added capability index entry.

## Findings
Current forecast can capitalise holiday interest and leave a separately reported terminal residual. The final-row statement in the schedule spec marked approved domain design conflicts with that code path. Explicit residual reporting does not select term extension, recast or a final lump sum.
Operational holiday effects must be investigated separately from L3 projection.

## Next discussion — ask these first
1. Q-004: Does the accepted holiday create a revised contractual schedule/version, or use another explicit agreement mechanism?
2. Q-002/Q-003: Agree exact repayment calculation and allocation of separate holiday interest versus current interest and principal.
3. Q-005: Confirm collection relief, new arrears/DPD and pre-existing arrears treatment.

Do not reopen D-002–D-006 without a conflict or new instruction. Resolve operational effects and exceptions afterwards. Updated FR-001–FR-007, BR-001–BR-003 and affected examples; no numerical outcomes calculated.

## Evidence freshness and validation
Platform evidence: `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`. Requirements instructions read at `9b086b4493966bd37e8640d33d9f3dd058ba6d3a`.
Source/test-definition inspection only. No runtime, UI or application tests executed. Numerical acceptance outcomes are not yet approved.
Before implementation, recheck affected source against the current platform head.

## Delivery handover
Requirements baseline: Not accepted or pinned for implementation.
Delivery state: Blocked; see [implementation.md](implementation.md).
