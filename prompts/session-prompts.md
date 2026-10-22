# Session prompts

## First session
Read AGENTS.md, project-context.md and the shared standards. Help me define [capability]. My intended outcome is [outcome]. These decisions are already made: [decisions or none]. Create its capability folder from the templates. Establish the brief, investigate relevant evidence available to you, and guide me through one to three related decisions at a time. Save confirmed decisions as we work. Do not invent business rules.

## Resume
Read AGENTS.md and resume [capability] from its requirements, decisions and checkpoint files. Briefly state agreed scope and the next unresolved decision. Check evidence freshness where needed. Continue without reopening confirmed decisions unless there is a conflict or I request a change.

## Checkpoint
Update the requirements, decisions and checkpoint for this capability. Show the decisions recorded, outstanding blockers, files changed and exact next step. Keep proposals visibly unconfirmed. Do not mark the specification ready without my acceptance.

## Review
Review [capability] against standards/review-checklist.md and writing-profile.md. Report ambiguity, missing material decisions, contradictions and unsupported assumptions. Propose edits without choosing unresolved business behaviour. Distinguish behaviour review from language review and executed tests.

## Change a decision
For [capability], I want to reconsider [decision ID]. Show affected requirements and examples. After I confirm the replacement, mark the previous decision superseded and update the affected records.

## Prepare platform handoff
Read standards/delivery-workflow.md. For [capability], verify my explicit acceptance and remaining blockers. Prepare implementation.md with scoped FR/BR/EX IDs, the accepted requirements commit, platform evidence commit, existing behaviour mappings and validation expectations. If approval or the baseline is missing, record that blocker. Do not start implementation or create platform tickets unless instructed.

## Implement approved requirements (use in loan-admin-platform)
Read this platform's AGENTS.md and follow its workflow. Implement [capability] from [immutable product_requirements commit URL / capability path]. Read the pinned requirements, decisions and examples; use implementation.md as the handoff. Compare the platform against evidence commit [SHA]. Surface material ambiguities and ADR conflicts before affected implementation. Follow the .scratch ticket and graph conventions, map requirement IDs to tests, and report exact changes, commands, results and limitations. Do not merge or deploy without authorisation.

## Review delivered behaviour
Compare [platform PR / commit] against [capability] at requirements baseline [SHA]. Map scoped requirements and examples to implementation and actual test evidence. Record gaps in implementation.md and checkpoint.md. Do not infer product acceptance from passing tests.
