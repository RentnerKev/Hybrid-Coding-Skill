# Orchestration

GPT-6.2 Sol MAX is the default lead. It owns the user request, repository constraints and conventions, architecture, delegation, integration, validation, and final decisions. Workers answer bounded questions or make isolated edits. The lead checks their evidence and remains responsible for the outcome.

## Model routing

Use GPT-6 Luna HIGH for inexpensive discovery or mechanical scopes: repository exploration, file and symbol discovery, call-site mapping, dependency tracing, configuration inspection, existing-pattern discovery, test discovery, simple checks, and mechanical edits with settled contracts.

Use GPT-6.2 Sol MEDIUM for substantive bounded work such as implementation, root-cause investigation, API contract analysis, planning, and regression review. Choose Sol HIGH when a specific question needs more reasoning and MAX when extra depth can change the result.

Use GPT-6.2 Astra for a hard question that focused Sol work leaves unresolved, or for an independent check where the consequences of error justify it. Examples include an intermittent cross-service failure, a subtle race, and a migration that could corrupt data. Choose Astra HIGH for a focused second opinion and Astra MAX when depth matters more than time or usage. A clearly demanding or high-impact question can go directly to Astra.

For an independent review, provide the relevant requirements, evidence, and diff. Ask for findings before sharing the lead's conclusions. Include verification, corrections, and coordination when deciding whether to delegate.

## Delegation guide

The ranges are planning guidance for the whole task, never quotas, minimums, or simultaneous worker targets. Respect the runtime concurrency limit and schedule dependent work in waves.

| Task size | Lead | Typical worker use | Specialist use |
| --- | --- | --- | --- |
| Trivial | GPT-6.2 Sol MAX | Usually none, with one Luna HIGH worker if discovery adds value | Usually none |
| Small | GPT-6.2 Sol MAX | Roughly 1–3 useful Luna HIGH or Sol MEDIUM scopes if they separate | Only for a concrete difficult or high-impact question |
| Medium | GPT-6.2 Sol MAX | Roughly 3–6 useful Luna HIGH or Sol MEDIUM scopes | Optional for a concrete difficult or high-impact question |
| Large | GPT-6.2 Sol MAX | Roughly 6–10 useful Luna HIGH or Sol MEDIUM scopes | Optional 1–2, each with a distinct difficult question |

Do not spawn merely to reach a count or because a model is inexpensive. A hard but poorly parallelizable task can have fewer workers than a straightforward migration with independent discovery scopes. Stop when another worker is unlikely to change the decision or evidence.

Give each worker a precise objective, bounded files or symbols, relevant repository constraints, whether edits are allowed, expected output, and a stopping condition. Ask for paths and symbols supporting findings, checks performed, assumptions, and unresolved uncertainty. Send the context needed for the assignment, not the entire repository or conversation.

Run independent reads, searches, and analyses in parallel when useful. Keep dependent work sequential: an implementer needs settled contracts, and a reviewer needs the actual implementation. For concurrent editing, assign disjoint files or modules and integrate through the lead. Do not duplicate work except for an intentionally independent second opinion.

For example, a medium feature may start with one Luna HIGH worker locating project patterns and another mapping callers and tests. After the lead settles the design, a Sol MEDIUM implementer may own isolated files. A Sol MEDIUM reviewer can inspect edge cases in the diff. If the change risks data loss during a migration, an Astra reviewer can independently check the data invariants and rollback path.

## Integration and verification

The lead checks worker claims against the cited repository evidence before acting. Resolve disagreements by reading the relevant code, reproducing behavior, or requesting a narrow follow-up. Escalate a remaining difficult question when deeper reasoning can change the result. The lead makes the final decision.

Use existing repository conventions and follow explicit user instructions plus applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, and project documentation. Keep the implementation within the requested scope and preserve user changes.

The lead implements directly where appropriate, integrates delegated changes, and runs relevant project tests, lint, type checks, builds, and repository-specific validation. Discover real commands from scripts, CI, or project docs. Fix failures caused by the work and distinguish pre-existing failures or unavailable checks. Avoid repeating successful checks unless code or evidence changed.

Finish by inspecting the complete diff for correctness, regressions, unrelated edits, private data, and unnecessary complexity. Apply the model routing above when an independent review is warranted. Resolve actionable findings and verify subsequent edits before reporting the result and limitations.
