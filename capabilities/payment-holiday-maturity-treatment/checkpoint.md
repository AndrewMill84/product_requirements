# Checkpoint — Payment-holiday maturity treatment

Last reviewed: 22 October 2026  
Status: Draft — reconciliation completed; final decisions and acceptance pending

## Current state
D-001–D-023 remain confirmed. FR-001–FR-041 and BR-001–BR-017 are preserved. Product direction is settled: separate non-interest-bearing holiday interest, increased equal repayments, unchanged maturity, future-effective contractual version, holiday DPD/penalty pause, old-debt retention, voluntary receipts and fresh operator preview.

## This review
Reconciled current code/tests/documents at platform `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d` against requirements input `ec94bbbf297bcade8d77bf9e7c2eacd7315b5fc0`.
Added [reconciliation-review.md](reconciliation-review.md), removed stale active options and repeated scope summaries, retained decision/requirement/example IDs and separated business blockers from technical work.
Actual auth/approval integration, version lifecycle, non-capitalising forecast, operational pause and bound preview are not implemented in the inspected paths. Daily actual rounding differs from period forecast rounding.

## Next discussion — four grouped decisions
1. R-001: Exact repayment component calculation and partial/voluntary interest attribution.
2. R-002/Q-008: Recovery of approved projected versus actual holiday-interest differences after approval.
3. R-003: Zero deferred scheduled collections, old-debt collection/principal partition and exceptional eligibility.
4. R-004: General permitted actors for preview/approval/cancellation; current API has no enforced mapping.

Concrete recommendations are in the review. Do not reopen term extension, capitalisation or other settled initial choices.

## Validation and handoff
No application/runtime/UI tests executed; source and test-definition inspection only. No local application environment was present.
Documentation IDs/links and remote saved contents are checked during writeback.
Requirements remain Draft, not accepted, not Ready for engineering and not pinned as an accepted implementation baseline. See [implementation.md](implementation.md). After decisions: complete money examples, obtain explicit final acceptance, then freeze a requirements commit.

## Process learning
Earlier reconciliation and grouped questions would have reduced repeated discussion. Resume with the four decision groups in [the review](reconciliation-review.md). Use one end-to-end scenario to check their interaction. Do not reopen confirmed policies.
