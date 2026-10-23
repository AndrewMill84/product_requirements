# Setup and daily use

## Open the partner repositories
Clone both repositories into a common parent folder:
```bash
git clone https://github.com/AndrewMill84/product_requirements.git
git clone https://github.com/AndrewMill84/loan-admin-platform.git
```
Open product_requirements in your agent tool and attach the sibling platform checkout for investigation, or use authorised GitHub read access. Confirm access and actual paths; do not assume the agent can see another folder.

## Load instructions
Ask the requirements agent to read AGENTS.md, project-context.md and shared standards. CLAUDE.md points Claude Code to AGENTS.md. No custom skill, system prompt, API key or application startup is required for this documentation workflow.

The platform's AGENTS.md applies when working there. Running application tests or the UI requires its documented environment; reading source does not require starting the application.

## First capability
For a simple change, copy templates/compact-requirements.md to capabilities/<slug>/requirements.md. Otherwise copy templates/capability/ to capabilities/<slug>/. Add a row to capabilities/README.md. Describe the outcome and known decisions. Ask the agent to investigate the affected platform areas at a recorded commit and resolve remaining questions with you.

## Deliver
Review requirements and examples. Once Andrew explicitly accepts the defined scope, commit and push that baseline. Record its full SHA in implementation.md or the compact specification's Implementation section in a subsequent commit, then pass the handoff to the implementation agent. A commit cannot contain its own final SHA: pin the earlier accepted content, not a moving main link.
Follow standards/delivery-workflow.md for tickets, traceability and verification.

## Continue across sessions
Save decisions during discussion and checkpoint before handover. Pull latest changes before resuming elsewhere. Avoid simultaneous edits to the same capability. Resolve conflicting decisions explicitly rather than allowing Git conflict resolution to choose business behaviour.
