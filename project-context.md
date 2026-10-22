# Project context

Owner and product decision authority: Andrew Millard
Requirements repository: https://github.com/AndrewMill84/product_requirements
Implementation repository: https://github.com/AndrewMill84/loan-admin-platform
Purpose confirmed by Andrew: Use this repository to create requirements that AI agents will implement in loan-admin-platform.
Business domain: Loan administration and servicing, including mortgage-related capabilities.
Jurisdictions, currencies and additional stakeholder authorities: Establish per capability.
Local checkouts: Environment-specific; suggested sibling folders product_requirements/ and loan-admin-platform/. A documented path does not grant access.

## Platform orientation
Reviewed 22 October 2026 at platform commit `1878f74d7c2a84392c7c5c2a5c9d702f7c06a36d`.
Sources: platform README.md, AGENTS.md, CONTEXT.md, docs/README.md, docs/agents/graph-workflow.md and docs/agents/issue-tracker.md.
This is documentation-based orientation, not an executed validation of runtime behaviour or a full code audit.

| Area | Platform reference |
| --- | --- |
| Domain calculations | packages/loan-core/ |
| Application commands, events and replay | packages/loan-application/ |
| Event schemas | packages/loan-schemas/ |
| Read projections | packages/loan-projections/ |
| API and servicing UI | services/api/ and frontend/ |
| Vocabulary and architecture decisions | CONTEXT.md and docs/adr/ |
| Behaviour navigation | .cockpit/graph/ and docs/agents/graph-workflow.md |
| Implementation tickets | .scratch/<feature-slug>/spec.md and issues/; see docs/agents/issue-tracker.md |
| Validation | tests/ and docs/agents/known-issues.md |

The platform describes event-sourced accounting and loan servicing. Recheck relevant code, tests and authoritative documents for each capability. The documentation may conflict or become stale; do not import financial rules from this overview.

## Authority and boundaries
This repository now explicitly targets Andrew's loan-admin-platform. It does not represent an employer's application by implication.
Confirmed requirements here define agreed target behaviour for their stated scope. Platform ADRs and specifications establish existing constraints and documented intent. Conflicts require an explicit decision and coordinated documentation change; this repository does not silently override locked ADRs.
Application AGENTS.md governs work in the platform checkout. This repository's AGENTS.md governs requirements work.
No financial product defaults are established merely by linking the repositories.

## Collaboration
Use reviewable commits and preserve existing work. Follow any later explicit branch or PR policy. Product acceptance does not itself authorise merging, deployment or unrelated platform changes.
See standards/delivery-workflow.md for the handoff contract.
