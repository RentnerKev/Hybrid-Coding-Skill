---
name: hybrid-coding
description: Use for coding tasks in Codex that benefit from cost-efficient multi-agent exploration, implementation, debugging, or review. Keep GPT-6 Astra MEDIUM as the lead, delegate bounded parallel work to GPT-5.6 Luna, and use GPT-5.6 Terra only for difficult specialist escalation.
---

# Hybrid Coding

Optimize coding quality and token/credit efficiency with one persistent lead and useful, bounded workers. Keep global context and decisions with GPT-6 Astra MEDIUM; delegate detail work when its value exceeds coordination cost.

## Model strategy

| Assignment | Preferred model | Reasoning | Selection rule |
| --- | --- | --- | --- |
| Lead and integration | GPT-6 Astra | MEDIUM | Default for every task size |
| Exploration and mechanical work | GPT-5.6 Luna | HIGH | File discovery, call sites, tests, dependencies, simple checks, small bounded edits |
| Bounded deep analysis | GPT-5.6 Luna | MAX | Bug analysis, implementation planning, edge cases, regressions, focused review |
| Difficult specialist work | GPT-5.6 Terra | MAX | Concrete concurrency, consistency, architecture, or other deep problem that warrants escalation |

Use the runtime's supported model selectors (for example, `gpt-6-astra`, `gpt-5.6-luna`, and `gpt-5.6-terra`) and effort controls. These are routing preferences, not configuration installed by this skill. Respect explicit user choices and actual tool capabilities. Do not claim to change the active lead or reasoning if no supported control exists. If a preferred worker is unavailable, disclose the limitation and use the least costly adequate available option; work locally if delegation is unavailable or unhelpful.

Keep Terra at zero by default. Use a targeted Terra MAX worker only for a concrete difficult question, such as unresolved Luna findings, a race condition, or subtle transaction invariants. Do not route routine exploration, CRUD, or ordinary review to Terra.

Escalate the active Astra lead from MEDIUM to HIGH, then XHIGH, then MAX only when an important unresolved decision exceeds the current effort. Task size alone is not a trigger. Do not spawn another Astra as a substitute for supported lead controls.

## Size and delegation

| Size | Examples | Luna worker guide |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS or config fix | Usually 0; optionally 1 HIGH |
| Small | Isolated bugfix, small component or endpoint | 1–3, mainly HIGH; MAX for real analysis |
| Medium | Feature, CRUD, module refactor, API + UI + tests | Approximately 3–6, mixing HIGH and MAX |
| Large | Repository-wide refactor, new subsystem, complex regression | Approximately 6–10, with independent exploration and review |

These are planning ranges with upper bounds, never spawn quotas. Use fewer workers when scopes do not separate. Count workers across the task, respect the runtime's concurrency limit, and schedule useful work in waves where necessary. Prefer independent Luna scopes before expensive escalation; stop spawning once further work is unlikely to change the outcome.

## Execution

1. Read the request, current Git state, and applicable repository instructions, including `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, README rules, and established code patterns. Preserve user changes and remain within the authorized scope.
2. Classify size, risk, and parallelism. Keep one coherent plan. Do trivial work directly; delegate independent read/search/analysis tasks where useful.
3. Give every worker an exact goal, scope, relevant files or symbols, edit permissions, expected result, and stopping condition. Request concise evidence with paths and symbols, checks performed, and explicit uncertainty. Prohibit scope expansion.
4. Keep concurrent writers on disjoint files or modules. Gather cross-cutting findings before implementation decisions. The lead implements or coordinates implementation and owns integration.
5. Synthesize findings against repository evidence. Resolve disagreements with a narrow follow-up, reproduction, or justified specialist; do not treat worker agreement as proof. Reuse existing workers and findings when practical.
6. Run relevant existing tests, lint, typecheck, and build where available. Discover real commands from project tooling. Fix failures caused by the change and distinguish pre-existing failures or unavailable checks.
7. Review the final diff for correctness, regressions, scope, accidental private data, and unnecessary complexity. Use a focused independent Luna MAX review when risk justifies it. Report the outcome, verification, and remaining limitations.

## References

- Read [orchestration](references/orchestration.md) when planning multiple workers, resolving conflicting findings, or considering escalation.
- Read [worker roles](references/roles.md) when selecting a role or drafting a bounded assignment.
