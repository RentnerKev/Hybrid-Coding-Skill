---
name: hybrid-coding-claude
description: Use in Claude Code for coding tasks that benefit from selective delegation for repository discovery, implementation, debugging, or review. The lead is Opus 5.5 at extra high effort, with native subagents for bounded assignments.
---

# Hybrid Coding for Claude Code

Keep the main conversation responsible for the user request, design decisions, delegation, integration, validation, and final review. Start it with `claude --model claude-opus-5-5 --effort xhigh`. Honor any explicit user choice of lead model or effort.

## Choose the subagent

Use the named plugin agents through Claude Code's Agent tool. Let each definition supply its model and effort. Set an effort override only when the deep reviewer needs more depth.

| Agent | Model and effort | Delegate when |
| --- | --- | --- |
| `hybrid-coding:explorer` | Haiku 5.5 `high` | A focused search for entry points, callers, dependencies, configuration, examples, or tests can save lead context. |
| `hybrid-coding:implementer` | Sonnet 5.5 `medium` | A scoped change, bug investigation, implementation plan, or review has a clear boundary. Pass explicit edit permission and file ownership for implementation. |
| `hybrid-coding:deep-reviewer` | Opus 5.5 `high` | A difficult but bounded diagnosis or review needs more reasoning. Raise effort to `xhigh` or `max` only when the added depth can change the result. |
| `hybrid-coding:specialist` | Fable 5.2 `xhigh` | An unresolved cross-module problem or a consequential independent review justifies a specialist. A clearly demanding question can go directly here. |

If Fable is unavailable or requires usage credits the user has not approved, use the Opus deep reviewer at `xhigh` or `max`, or handle the question in the main conversation. If other named agents or the Agent tool are unavailable, do the work directly or use a supported equivalent for the same bounded role. Report material substitutions. Respect the account's model availability, session overrides, and permissions.

## Delegate and verify

1. Inspect the request, Git state, repository instructions, and relevant code. For a small edit with a clear path, work directly. For broader tasks, split only genuinely independent scopes and use the fewest workers that add useful evidence or speed.
2. Give each worker one objective, the relevant files or symbols, the constraints, its edit boundary, and the expected evidence or deliverable. For an independent review, provide the requirements and diff before sharing your conclusions.
3. Run independent assignments in parallel when useful. Give simultaneous writers disjoint file ownership. Start dependent work after the input it needs is ready. Keep track of asynchronous workers and collect their results before relying on them.
4. Verify findings against the repository, reconcile conflicting results, and integrate scoped edits yourself. Run the relevant project checks and review the complete diff for regressions, scope, and correctness. Report what changed, what passed, and any verification limits.
