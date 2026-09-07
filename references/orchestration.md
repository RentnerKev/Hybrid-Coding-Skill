# Orchestration

Use a strong lead and narrowly scoped workers. The lead owns the user request, repository context, architecture, delegation, integration, verification, and final response. Workers supply bounded evidence or implementation help; the lead remains responsible for judging their results.

## Model routing

The default lead is Astra at MEDIUM reasoning. Start every task there. Astra MEDIUM should classify the work, inspect enough context to plan safely, decide whether delegation adds value, assign independent scopes, synthesize findings, integrate changes, and review the final result.

Use Luna at HIGH for read-heavy and mechanically bounded work: repository exploration, symbol and call-site mapping, dependency tracing, configuration and pattern discovery, test discovery, API or type inventory, documentation lookup within the project, and simple regression checks. Use Luna at MAX for bounded work where deeper reasoning can change the result: root-cause analysis, edge cases, regression review, implementation-path comparison, adversarial review, test-plan design, migration analysis, or a tightly scoped implementation.

Use Terra at MAX only for a concrete difficult subproblem. Valid triggers include unresolved disagreement between independent Luna analyses, an ambiguous root cause after focused investigation, an invariant spanning several modules, difficult transaction or consistency reasoning, concurrency or race-condition analysis, a risky implementation needing an independent deep review, or an algorithmic or compiler problem beyond Luna's confidence. Default Terra usage is zero; target at most one worker. Do not use Terra for routine exploration, formatting, test discovery, straightforward CRUD, or ordinary frontend work.

Increase the lead's Astra reasoning only when MEDIUM cannot safely resolve an important, high-impact decision. Escalate progressively from HIGH to XHIGH to MAX. The trigger is unresolved reasoning risk, such as architecture, security, concurrency, a major migration, conflicting evidence, or an irreversible choice; task size alone is not a reason to escalate. Model and effort selections are preferences subject to the runtime's available models and controls. Never claim to have switched the active lead without a supported control. If a preferred selection is unavailable, use the least costly adequate supported route and state any material limitation. Explicit user choices take precedence.

## Delegation guide

These are ceilings and planning guidance, not quotas. Counts are for the whole task, not simultaneous workers, and always yield to the runtime concurrency limit.

| Task size | Lead | Typical Luna use | Terra |
| --- | --- | --- | --- |
| Trivial | Astra MEDIUM | 0; optionally 1 HIGH verification worker when regression risk exists | 0 |
| Small | Astra MEDIUM | 1–3 workers, usually HIGH; MAX only for one bounded analytical question | 0 |
| Medium | Astra MEDIUM | Approximately 3–6 workers, commonly 2–3 HIGH plus 1–2 MAX | 0 by default; at most 1 after a concrete trigger |
| Large | Astra MEDIUM | Approximately 6–10 workers, commonly 3–4 HIGH plus 3–4 MAX | 0 by default; at most 1 after a concrete trigger |

Do not spawn merely to reach a count. A poorly parallelizable deep task usually needs Astra MEDIUM, one justified Terra MAX specialist, and a small number of Luna evidence or verification workers. Stop spawning when another worker is unlikely to change the decision or evidence.

Give each worker an independent question and explicit boundaries. Prefer read-only investigations before implementation. For implementation work, assign exact file or module ownership and never allow concurrent edits to the same files unless the lead has deliberately planned reconciliation. Do not duplicate work except for an intentional independent review. Prompts should state the goal, scope, whether edits are allowed, relevant files, expected evidence, and that uncertainty must be reported instead of guessed.

Run independent reads, searches, and analyses in parallel within the available slots. Keep dependent work sequential: a reviewer needs the proposed implementation, and an implementer needs any unresolved contract decision settled first. The lead should continue useful independent work while workers run.

For example, a medium feature may start with one Luna HIGH mapping project patterns and another locating callers and tests. After synthesis, assign disjoint implementation work where useful and use Luna MAX to review the resulting edge cases. Do not launch every role in the catalog for each feature.

## Token and credit discipline

Keep the full task context with the lead. Send workers only the relevant constraints, files, and questions; do not copy the full repository or conversation into every prompt. Ask for findings, supporting locations, checks, and unresolved questions rather than long investigation transcripts.

Reuse findings and existing workers for narrow follow-ups. Use HIGH when search or mechanical work answers the question and MAX when deeper reasoning could change the result. Prefer independent Luna scopes over expensive parallel workers when the task separates cleanly. Stop obsolete investigations and avoid repeating successful checks unless changes or new evidence justify it. Judge efficiency by useful decisions and verified outcomes, not agent count; no fixed savings are guaranteed.

## Synthesis and verification

Collect the relevant findings before escalating to a more expensive worker. The lead should compare evidence, record meaningful uncertainty, choose one coherent implementation path, and explain any unresolved limitation. Workers should report concise evidence with paths and symbols rather than broad summaries.

Resolve conflicting findings by inspecting the cited code or reproducing the behavior, then assign a narrow follow-up if needed. Escalate to Terra only when the remaining uncertainty is a difficult specialist question. The lead owns the decision and must not accept a majority vote as verification.

The lead integrates all changes and runs the project's existing relevant tests, lint, typecheck, and build where available. Inspect project scripts, task runners, CI configuration, or tooling before selecting checks; do not invent commands. Fix failures caused by the change, rerun affected checks, and distinguish pre-existing failures from unavailable validation.

Finish with a focused diff review for correctness, regressions, security issues, scope creep, private data, and unnecessary complexity. For meaningful behavioral risk, give a Luna MAX reviewer the actual diff and affected contracts; trivial edits normally need only the lead's review. Resolve actionable findings and verify resulting edits before reporting completion. A worker's passing check is evidence, not a substitute for the lead's final review.
