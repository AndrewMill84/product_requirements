# Checkpoint — Payment-holiday maturity treatment

Last updated: 22 October 2026  
Status: Draft discovery; awaiting product decisions

## Agreed task
D-001 authorises expert-led draft requirements for the recommended capability. No lending policy or release scope accepted.

## Progress
Read the requirements workflow and actual platform Loan Domain Expert skill/prompt. Inspected graph, ADRs, schedule specification, holiday code, test definitions and related tickets.
Created requirements.md, decisions.md and implementation.md. Added capability index entry.

## Findings
Current forecast can capitalise holiday interest and leave a separately reported terminal residual. The final-row statement in the schedule spec marked approved domain design conflicts with that code path. Explicit residual reporting does not select term extension, recast or a final lump sum.
Operational holiday effects must be investigated separately from L3 projection.

## Next discussion — ask these first
1. Q-001: Is the proposed first slice one future holiday on an active fixed-rate annuity, including actual servicing effects?
2. Q-002: Should interest continue, and if so remain separate or be capitalised? Is that choice configurable?
3. Q-003: Should deferred debt extend the term, increase subsequent instalments, or be collected in a final lump sum?

After those answers, update decisions and affected candidates/examples, then resolve contractual, operational and exception questions Q-004–Q-007.

## Evidence freshness and validation
Platform evidence: `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`. Requirements instructions read at `9b086b4493966bd37e8640d33d9f3dd058ba6d3a`.
Source/test-definition inspection only. No runtime, UI or application tests executed. Numerical acceptance outcomes are not yet approved.
Before implementation, recheck affected source against the current platform head.

## Delivery handover
Requirements baseline: Not accepted or pinned for implementation.
Delivery state: Blocked; see [implementation.md](implementation.md).
