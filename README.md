# Product requirements — Loan Administration Platform

This is the requirements partner repository for [loan-admin-platform](https://github.com/AndrewMill84/loan-admin-platform). Andrew and an AI requirements partner use this workspace to define concise, confirmed functional behaviour before an implementation agent changes the platform.

## Two repositories, one delivery flow
| Repository | Responsibility |
| --- | --- |
| [product_requirements](https://github.com/AndrewMill84/product_requirements) | Capability discovery, product decisions, functional requirements, acceptance examples and handoff references |
| [loan-admin-platform](https://github.com/AndrewMill84/loan-admin-platform) | Implementation, architecture decisions, domain graph, local implementation tickets, tests and delivery evidence |

Describe outcome → investigate platform → resolve decisions → review examples → Andrew accepts scope → pin requirements commit → implement and test in platform → record results here.

A committed draft is not approval. Product acceptance and implementation verification are separate.

## Start here
1. Read [AGENTS.md](AGENTS.md), [project-context.md](project-context.md) and [SETUP.md](SETUP.md).
2. Give the agent read access to the platform repository.
3. Choose the [compact template](templates/compact-requirements.md) for simple changes or copy [templates/capability/](templates/capability/) for capabilities that benefit from separate records. Store either under `capabilities/<slug>/` and add it to the [index](capabilities/README.md).
4. Use [session prompts](prompts/session-prompts.md) to discover or resume requirements.
5. Follow [the delivery workflow](standards/delivery-workflow.md) when handing approved work to an implementation agent.

## Records
There is no page limit. Scale detail and structure to the change; never remove necessary behaviour to shorten a specification.

Each capability records requirements, decisions and a checkpoint as sections or separate files. Add the implementation handoff before delivery. The handoff links a fixed specification version to platform tickets, PRs and test evidence. Keep functional rules here; reference existing platform specifications rather than copying them into competing documents.

## Record responsibilities

Scale the record structure to the change. A simple change may contain requirements, decisions and a checkpoint in one file. Use separate linked records when they improve clarity, preserve substantial decision history or support implementation handoff. Do not create empty documents merely to satisfy a template. Preserve stable IDs and update links when splitting records.

| Record or section | Authoritative content |
| --- | --- |
| Requirements | Current required behaviour, scope, exceptions and acceptance examples |
| Decisions | Choices, rationale, unresolved questions and superseded alternatives |
| Checkpoint | Current progress and the exact next step |
| Implementation | Accepted baseline, delivery references and verification coverage |
| Investigation/review | Source evidence, findings and discrepancies |

Maintain each substantive fact in one authoritative location. Elsewhere, use a brief orientation summary and a link. When a confirmed decision changes, reconcile affected requirements and examples. Requirements express the selected behaviour; decisions explain its authority and rationale.

The product manager owns new business decisions. Existing platform ADRs and vocabulary remain applicable unless an explicit change is agreed and reconciled. All files under examples/ remain fictional and unapproved.

Writing uses an adapted ASD-STE100-based profile; full compliance is not claimed. No application code or AI service is included.
