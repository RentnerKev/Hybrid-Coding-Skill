---
name: hybrid-coding
description: Use for coding tasks in Codex that benefit from selective multi-agent exploration, implementation, debugging, or review. Keep GPT-6.2 Sol MAX as the lead, use GPT-6 Luna HIGH for discovery, GPT-6.2 Sol MEDIUM for substantive work, and GPT-6.2 Astra for the hardest specialist questions.
---

# Hybrid Coding

Keep GPT-6.2 Sol MAX responsible for repository context, orchestration, implementation decisions, integration, validation, and final review. Delegate a bounded assignment when its value justifies the coordination cost.

## Model strategy

| Assignment | Preferred model | Reasoning | Selection rule |
| --- | --- | --- | --- |
| Persistent lead, integration, final decisions | GPT-6.2 Sol | MAX | Keep ownership for every task size |
| Repository exploration and mechanical analysis | GPT-6 Luna | HIGH | File and call-site discovery, tests, dependencies, configuration, existing patterns |
| Bounded implementation, analysis, and review | GPT-6.2 Sol | MEDIUM | Substantive code changes, bug investigation, planning, API contracts, edge cases, regressions, debugging, and review |
| Difficult bounded question | GPT-6.2 Sol | HIGH/MAX | Use HIGH when the scope calls for deeper reasoning. Use MAX when extra depth can change the decision. |
| Hardest specialist questions or high-impact second opinions | GPT-6.2 Astra | HIGH/MAX | Use for unresolved cross-module failures, difficult invariants, or an independent review where a missed defect has serious consequences. |

Use the supported runtime selectors `gpt-6.2-sol`, `gpt-6-luna`, and `gpt-6.2-astra` with the stated reasoning effort. Select the lead model and effort in Codex when starting a task. Respect explicit user model choices and runtime capabilities. If Astra is unavailable, use a supported Sol HIGH/MAX worker or handle the question with the lead. Disclose material substitutions and continue directly when delegation is unavailable.

The default lead uses MAX even for a trivial task. Control cost by delegating fewer scopes and choosing worker effort based on the reasoning needs, consequences of errors, and verification effort. Route a clearly difficult or high-impact question directly to the appropriate specialist. File count and task size alone do not justify a specialist. Choose Ultra deliberately for work that benefits from automatic delegation. Codex can proactively spawn subagents in that mode.

## Size and delegation

| Size | Examples | Typical guidance for the whole task |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS or config fix, tiny one-file edit | Usually no worker; optionally one Luna HIGH for useful discovery |
| Small | Isolated bugfix, small component or endpoint, focused test change | Roughly 1–3 useful Luna HIGH or Sol MEDIUM scopes. Use a specialist for a concrete difficult question. |
| Medium | Feature, CRUD flow, module refactor, API + UI + tests | Roughly 3–6 useful Luna HIGH or Sol MEDIUM scopes, with a specialist if justified |
| Large | Broad refactor, repository-wide migration, new subsystem, complex regression | Roughly 6–10 useful Luna HIGH or Sol MEDIUM scopes, with 1–2 specialists if justified |

These ranges are guidelines, never quotas or minimums. Use fewer workers when scopes do not separate. Counts cover the entire task, not simultaneous agents; follow runtime concurrency limits and run dependent work in sequence. Stop delegating when another worker is unlikely to change the decision or evidence.

## Execution

1. Understand the request and current Git state. Read applicable `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, project documentation, and relevant code conventions. Explicit user instructions and repository constraints govern the work; preserve user changes and scope.
2. Decide whether delegation adds value. Keep the global plan and repository context with the Sol lead. Perform trivial work directly when discovery is unnecessary.
3. Give each worker a narrow objective, relevant context and files, edit permission or read-only boundary, expected output, and stopping condition. Ask for concise evidence with paths or symbols, checks performed, and uncertainty. Avoid duplicate assignments except for a deliberate independent second opinion.
4. Parallelize independent searches, analyses, and disjoint edits where useful. Keep dependent steps sequential. Assign exclusive file or module ownership to concurrent writers; the lead implements directly where appropriate, coordinates delegated edits, and integrates the result.
5. Verify worker findings against repository evidence before applying them. Resolve conflicts through inspection, reproduction, or a narrow follow-up. Use the model strategy for any specialist question and make the final decision as lead.
6. Run relevant existing tests, lint, type checks, builds, and repository-specific validation. Discover the commands from project tooling; fix failures caused by the change and distinguish pre-existing or unavailable checks.
7. Review the complete diff for correctness, regressions, unrelated changes, private data, and unnecessary complexity. Delegate an independent review when it can expose material issues. Report the result, validation, and material limitations.

## References

- Read [orchestration](references/orchestration.md) when planning multiple workers, choosing their model and effort, integrating conflicting findings, or considering a specialist.
- Read [worker roles](references/roles.md) when choosing roles or writing bounded worker assignments.
