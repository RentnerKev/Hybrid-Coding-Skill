# Worker roles

Choose the smallest set of roles that adds independent value. Each role below includes its normal model and effort, when it helps, and when it should be skipped. The lead may combine roles when their scopes remain clear.

| Role | Purpose | Default model and effort | Use when | Skip when |
| --- | --- | --- | --- | --- |
| repository-explorer | Map relevant directories, modules, symbols, conventions, and entry points. | Luna HIGH | The affected area or existing pattern is unclear. | The change is confined to a file the lead already understands. |
| call-site-analyzer | Find callers, consumers, routes, event paths, and contracts affected by a symbol or API. | Luna HIGH | A signature, behavior, or shared component may have downstream users. | No reusable symbol or consumer boundary exists. |
| test-analyzer | Locate tests, fixtures, scripts, and coverage gaps for the affected behavior. | Luna HIGH | The repository has existing tests or the change has meaningful regression risk. | A trivial, self-evident edit has no relevant check. |
| dependency-analyzer | Trace package, module, service, schema, configuration, and version relationships. | Luna HIGH | Behavior depends on an unfamiliar dependency or cross-package contract. | The change uses a well-understood local dependency with no contract impact. |
| implementation-planner | Compare bounded implementation paths and identify files, invariants, and tradeoffs before editing. | Luna MAX | Several plausible paths exist or the change crosses a small module boundary. | The implementation is an obvious, local edit. |
| bug-investigator | Build and test root-cause hypotheses from control flow, state, inputs, and failure evidence. | Luna MAX | The symptom has more than one plausible cause. | The cause is directly established by a failing line or clear reproduction. |
| edge-case-reviewer | Examine boundaries, empty or malformed inputs, lifecycle transitions, error handling, and compatibility behavior. | Luna MAX | The change alters state, parsing, validation, or user-visible behavior. | The change cannot affect behavior beyond a mechanical rename or format fix. |
| regression-reviewer | Assess changed and neighboring paths for behavior regressions and propose focused checks. | Luna MAX | A fix or refactor touches shared logic or multiple consumers. | The lead has strong local evidence and the impact is genuinely isolated. |
| adversarial-reviewer | Try to falsify the proposed design through hostile inputs, misuse, security assumptions, and failure paths. | Luna MAX | The change has security, permissions, data-integrity, or high-impact behavior. | The task is low-risk and purely mechanical. |
| concurrency-specialist | Analyze interleavings, races, locks, retries, ordering, cancellation, and consistency under concurrent execution. | Terra MAX | Concurrency or a subtle cross-request consistency invariant is a concrete risk. | The code is single-threaded or has no shared mutable state; use Luna for ordinary async flow. |
| database-specialist | Review transactions, constraints, query semantics, indexes, migration safety, and consistency boundaries. | Terra MAX | A database change or data-integrity decision has non-trivial transactional consequences. | The task is a local query or schema read with obvious semantics. |
| bounded-implementer | Make a narrowly scoped change under explicit file ownership and report the diff and checks. | Luna HIGH for small mechanical edits; Luna MAX for bounded implementation reasoning | The lead has settled the design and the edit is isolated enough to delegate safely. | Requirements or architecture remain unsettled, or files overlap another writer. |

Here Astra means GPT-6 Astra; Luna and Terra mean GPT-5.6 Luna and GPT-5.6 Terra. Astra MEDIUM remains the default lead for every role combination.

Role defaults are routing preferences, not guarantees about runtime availability. The lead should use the least costly adequate supported alternative and disclose material limitations. Terra roles are exceptional specialist assignments, not a general substitute for exploration or review. Every worker must stay within its assigned scope and report evidence, assumptions, and unresolved questions concisely.
