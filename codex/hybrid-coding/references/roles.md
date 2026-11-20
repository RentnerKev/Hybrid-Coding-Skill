# Worker roles

Use these prompt patterns for workers that add independent value. Give each worker a boundary, relevant context, edit permission, expected output, and a stopping condition. Ask for concise evidence and uncertainty. Follow [model routing](orchestration.md#model-routing) when choosing Sol or Astra for a specialist question.

| Role | Recommended model and effort | Bounded objective | Context to provide | Expected output |
| --- | --- | --- | --- | --- |
| `repository-explorer` | GPT-6 Luna HIGH | Map the named subsystem's entry points, conventions, and relevant symbols. | User goal, starting paths or search terms, applicable project docs. | Key paths and symbols, established patterns, unresolved locations. |
| `call-site-analyzer` | GPT-6 Luna HIGH | Find consumers and contracts affected by one named API, symbol, or behavior. | Changed symbol or contract, affected package boundary. | Call sites with paths, contract assumptions, likely impact. |
| `test-analyzer` | GPT-6 Luna HIGH | Locate relevant tests, fixtures, scripts, and existing coverage gaps. | Target behavior, affected paths, project test tooling. | Relevant tests and commands, focused gaps, no invented checks. |
| `dependency-analyzer` | GPT-6 Luna HIGH | Trace dependencies and configuration connected to one named change. | Affected module, package or service names, relevant manifests. | Dependency map, version or configuration constraints, supporting paths. |
| `implementation-planner` | GPT-6.2 Sol MEDIUM | Compare plausible ways to implement a bounded feature or fix. | Requirements, relevant files, existing conventions, known constraints. | Recommended path, affected files, invariants, tradeoffs, open questions. |
| `bug-investigator` | GPT-6.2 Sol MEDIUM | Test root-cause hypotheses for a specific symptom or failing case. | Reproduction or logs, affected flow, relevant code and tests. | Evidence-backed cause or narrowed hypotheses, reproduction and checks. |
| `edge-case-reviewer` | GPT-6.2 Sol MEDIUM | Examine boundaries and failure paths of a proposed change. | Proposed behavior or diff, input and lifecycle contracts. | Concrete edge cases with locations and focused verification ideas. |
| `regression-reviewer` | GPT-6.2 Sol MEDIUM | Check changed and neighboring paths for unintended behavior. | Actual diff, existing behavior, known consumers and tests. | Actionable regressions with evidence and focused checks. |
| `adversarial-reviewer` | GPT-6.2 Sol MEDIUM, with specialist routing for difficult or high-impact questions | Challenge security, permission, data-integrity, and misuse assumptions in a bounded change. | Threat or misuse boundary, actual design or diff, relevant contracts. | Reproducible failure paths or evidence-backed risk findings. |
| `concurrency-specialist` | Sol or Astra HIGH/MAX | Resolve a concrete difficult race, ordering, retry, cancellation, or shared-state question. | Specific interleaving or invariant, relevant state and observations. | Reasoned failure sequence or invariant, minimal fix or verification plan. |
| `database-specialist` | Sol or Astra HIGH/MAX | Resolve a non-obvious transaction, constraint, migration, query, or consistency question. | Schema, query or migration, data-integrity requirement, relevant observations. | Contract-level conclusion, affected operations, validation or rollback concern. |
| `architecture-specialist` | Sol or Astra HIGH/MAX | Analyze a difficult cross-module design or invariant with competing choices. | System boundaries, requirements, relevant modules, relevant observations. | Decision options, invariants, tradeoffs, recommended narrow path. |
| `bounded-implementer` | GPT-6 Luna HIGH for settled mechanical edits or GPT-6.2 Sol MEDIUM for substantive implementation | Implement the settled design within exclusive file or module ownership. | Accepted plan, exact files, coding conventions, relevant checks. | Scoped diff summary, checks run, assumptions and unresolved issues. |

Give concurrent writers disjoint scopes. Use overlapping analysis only for a deliberate independent second opinion. The lead verifies worker findings and integrates the final implementation.
