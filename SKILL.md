---
name: hybrid-coding
description: Use for coding tasks in Codex that benefit from selective multi-agent exploration, implementation, debugging, or review. Keep GPT-6 Sol MAX as the persistent lead, route bounded work to GPT-6 Luna HIGH/MAX, and reserve GPT-6 Sol MAX specialists for difficult problems.
---

# Hybrid Coding

Keep GPT-6 Sol MAX responsible for the whole coding task: repository context, orchestration, implementation decisions, integration, validation, and final review. Use GPT-6 Luna workers only when a bounded assignment adds value beyond its coordination cost. An additional GPT-6 Sol MAX worker is a rare specialist, never a replacement lead.

## Model strategy

| Assignment | Preferred model | Reasoning | Selection rule |
| --- | --- | --- | --- |
| Persistent lead, integration, final decisions | GPT-6 Sol | MAX | Keep ownership for every task size |
| Repository exploration and mechanical analysis | GPT-6 Luna | HIGH | File and call-site discovery, tests, dependencies, configuration, existing patterns |
| Bounded deep analysis and review | GPT-6 Luna | MAX | Bug investigation, planning, API contracts, edge cases, regressions, debugging, adversarial review |
| Difficult specialist question | GPT-6 Sol | MAX | Complex concurrency, database behavior, architecture, cross-module invariants, subtle state or performance problems, or a high-risk second opinion when Luna is insufficient |

Use supported runtime selectors such as `gpt-6-sol` and `gpt-6-luna` with the stated reasoning effort. These are routing instructions, not configuration installed by this skill. For the documented setup, start the active Codex task with GPT-6 Sol at MAX; do not claim to change the active model or effort without a supported control. Respect explicit user model choices and actual runtime capabilities. If a preferred model or delegation is unavailable, explain the material limitation and continue useful work locally where possible. Do not silently substitute another model as the normal strategy.

The Sol lead stays at MAX even for a trivial task. Control cost by reducing worker use, assigning mechanical work to Luna HIGH, assigning deeper bounded reasoning to Luna MAX, and adding a Sol MAX specialist only for a concrete difficult question. File count and task size alone do not justify a specialist. Do not create redundant workers because Luna is inexpensive.

## Size and delegation

| Size | Examples | Typical guidance for the whole task |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS or config fix, tiny one-file edit | Usually no worker; optionally one Luna HIGH for useful discovery |
| Small | Isolated bugfix, small component or endpoint, focused test change | Roughly 1–3 Luna HIGH/MAX workers; Sol specialist only if unexpectedly difficult |
| Medium | Feature, CRUD flow, module refactor, API + UI + tests | Roughly 3–6 useful Luna HIGH/MAX scopes; optional Sol MAX specialist |
| Large | Broad refactor, repository-wide migration, new subsystem, complex regression | Roughly 6–10 useful Luna HIGH/MAX scopes; optional 1–2 Sol MAX specialists |

These ranges are guidelines, never quotas or minimums. Use fewer workers when scopes do not separate. Counts cover the entire task, not simultaneous agents; follow runtime concurrency limits and run dependent work in sequence. Stop delegating when another worker is unlikely to change the decision or evidence.

## Execution

1. Understand the request and current Git state. Read applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, project documentation, and relevant code conventions. Explicit user instructions and repository constraints govern the work; preserve user changes and scope.
2. Decide whether delegation adds value. Keep the global plan and repository context with the Sol lead. Perform trivial work directly when discovery is unnecessary.
3. Give each worker a narrow objective, relevant context and files, edit permission or read-only boundary, expected output, and stopping condition. Ask for concise evidence with paths or symbols, checks performed, and uncertainty. Avoid duplicate assignments except for a deliberate independent second opinion.
4. Parallelize independent searches, analyses, and disjoint edits where useful. Keep dependent steps sequential. Assign exclusive file or module ownership to concurrent writers; the lead implements directly where appropriate, coordinates delegated edits, and integrates the result.
5. Verify worker findings against repository evidence before applying them. Resolve conflicts through inspection, reproduction, or a narrow follow-up. Use an additional Sol MAX specialist only when a concrete difficult question remains beyond Luna's investigation; the lead makes the final decision.
6. Run relevant existing tests, lint, type checks, builds, and repository-specific validation. Discover the commands from project tooling; fix failures caused by the change and distinguish pre-existing or unavailable checks.
7. Review the complete diff for correctness, regressions, unrelated changes, private data, and unnecessary complexity. Use a focused Luna MAX independent review when risk warrants it. Report the result, validation, and material limitations.

## References

- Read [orchestration](references/orchestration.md) when planning multiple workers, integrating conflicting findings, or considering a Sol specialist.
- Read [worker roles](references/roles.md) when choosing roles or writing bounded worker assignments.
