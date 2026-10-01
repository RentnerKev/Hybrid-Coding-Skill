---
name: hybrid-coding
description: Use for coding tasks in Codex that benefit from selective multi-agent exploration, implementation, debugging, or review. Keep GPT-6.1 Sol MAX as the persistent lead, use GPT-6 Luna HIGH for discovery and mechanical work, and route substantive work to GPT-6.1 Sol MEDIUM with HIGH/MAX for difficult questions.
---

# Hybrid Coding

Keep GPT-6.1 Sol MAX responsible for the whole coding task: repository context, orchestration, implementation decisions, integration, validation, and final review. Use GPT-6 Luna HIGH for discovery and settled mechanical work, and GPT-6.1 Sol MEDIUM for substantive implementation, analysis, and review. A worker must add value beyond its coordination cost. An additional GPT-6.1 Sol HIGH/MAX worker answers a difficult bounded question, never replaces the lead.

## Model strategy

| Assignment | Preferred model | Reasoning | Selection rule |
| --- | --- | --- | --- |
| Persistent lead, integration, final decisions | GPT-6.1 Sol | MAX | Keep ownership for every task size |
| Repository exploration and mechanical analysis | GPT-6 Luna | HIGH | File and call-site discovery, tests, dependencies, configuration, existing patterns |
| Bounded implementation, analysis, and review | GPT-6.1 Sol | MEDIUM | Substantive code changes, bug investigation, planning, API contracts, edge cases, regressions, debugging, and review |
| Difficult specialist question or high-impact review | GPT-6.1 Sol | HIGH/MAX | Complex concurrency, database behavior, architecture, cross-module invariants, subtle state or performance problems, or a high-risk second opinion; choose HIGH first and MAX when the question warrants deeper reasoning |

Use supported runtime selectors such as `gpt-6.1-sol` and `gpt-6-luna` with the stated reasoning effort. These are routing instructions, not configuration installed by this skill. For the documented setup, start the active Codex task with GPT-6.1 Sol at MAX; do not claim to change the active model or effort without a supported control. Respect explicit user model choices and actual runtime capabilities. If a preferred model or delegation is unavailable, explain the material limitation and continue useful work locally where possible. Do not silently substitute another model as the normal strategy.

The Sol lead stays at MAX even for a trivial task. Control cost by reducing worker use, assigning discovery and settled mechanical work to Luna HIGH, and starting substantive workers at Sol MEDIUM. Route complex or high-impact questions directly to Sol HIGH/MAX when warranted. Choose the route by reasoning needs and the consequence of errors, including lead verification and rework, rather than token price alone. File count and task size alone do not justify a specialist. Do not create redundant workers because a model is inexpensive.

## Size and delegation

| Size | Examples | Typical guidance for the whole task |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS or config fix, tiny one-file edit | Usually no worker; optionally one Luna HIGH for useful discovery |
| Small | Isolated bugfix, small component or endpoint, focused test change | Roughly 1–3 useful Luna HIGH or Sol MEDIUM scopes; HIGH/MAX only for a concrete difficult question |
| Medium | Feature, CRUD flow, module refactor, API + UI + tests | Roughly 3–6 useful Luna HIGH or Sol MEDIUM scopes; optional Sol HIGH/MAX specialist |
| Large | Broad refactor, repository-wide migration, new subsystem, complex regression | Roughly 6–10 useful Luna HIGH or Sol MEDIUM scopes; optional 1–2 Sol HIGH/MAX specialists |

These ranges are guidelines, never quotas or minimums. Use fewer workers when scopes do not separate. Counts cover the entire task, not simultaneous agents; follow runtime concurrency limits and run dependent work in sequence. Stop delegating when another worker is unlikely to change the decision or evidence.

## Execution

1. Understand the request and current Git state. Read applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, project documentation, and relevant code conventions. Explicit user instructions and repository constraints govern the work; preserve user changes and scope.
2. Decide whether delegation adds value. Keep the global plan and repository context with the Sol lead. Perform trivial work directly when discovery is unnecessary.
3. Give each worker a narrow objective, relevant context and files, edit permission or read-only boundary, expected output, and stopping condition. Ask for concise evidence with paths or symbols, checks performed, and uncertainty. Avoid duplicate assignments except for a deliberate independent second opinion.
4. Parallelize independent searches, analyses, and disjoint edits where useful. Keep dependent steps sequential. Assign exclusive file or module ownership to concurrent writers; the lead implements directly where appropriate, coordinates delegated edits, and integrates the result.
5. Verify worker findings against repository evidence before applying them. Resolve conflicts through inspection, reproduction, or a narrow follow-up. Use an additional Sol HIGH/MAX specialist when a concrete question requires deeper reasoning or a high-risk second opinion; the lead makes the final decision.
6. Run relevant existing tests, lint, type checks, builds, and repository-specific validation. Discover the commands from project tooling; fix failures caused by the change and distinguish pre-existing or unavailable checks.
7. Review the complete diff for correctness, regressions, unrelated changes, private data, and unnecessary complexity. Use a focused Sol MEDIUM independent review when useful, or HIGH/MAX for a concrete difficult or high-impact question. Report the result, validation, and material limitations.

## References

- Read [orchestration](references/orchestration.md) when planning multiple workers, choosing their model and effort, integrating conflicting findings, or considering a Sol specialist.
- Read [worker roles](references/roles.md) when choosing roles or writing bounded worker assignments.
