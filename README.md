# Hybrid Coding Skill

<!-- [![skills.sh](https://skills.sh/b/RentnerKev/Hybrid-Coding-Skill)](https://skills.sh/RentnerKev/Hybrid-Coding-Skill) -->

Cost-efficient multi-agent coding orchestration for Codex.

**GPT-6 Sol MAX** owns the task as the persistent lead and final decision-maker. **GPT-6 Luna HIGH/MAX** workers handle useful, bounded exploration, analysis, testing, debugging, and review. Additional **GPT-6 Sol MAX** specialists are reserved for genuinely difficult questions.

The goal is high coding quality without unnecessary worker or context cost.

## Installation

### Bun

```powershell
bunx skills add RentnerKev/Hybrid-Coding-Skill --skill hybrid-coding -g -a codex
```

Verify the installation:

```powershell
bunx skills list -g
```

### Update

```powershell
bunx skills update hybrid-coding -g
```

Check all global skills for updates:

```powershell
bunx skills check -g
```

## Usage

Select the active model and reasoning level in Codex:

```text
GPT-6 Sol
Reasoning: MAX
```

Then invoke the skill with your task:

```text
$hybrid-coding

Implement the requested feature, preserve the existing architecture,
run the relevant checks, and keep the change focused.
```

GPT-6 Sol MAX remains the lead throughout the task. The skill can guide delegation but cannot change the active Codex model or unlock unavailable models or agents.

## Why this setup?

Sol provides strong repository-level coding, integration, and final judgment. Luna is capable of most bounded worker tasks at much lower cost, so exploration and review can run in parallel when their findings will help. A second Sol worker is useful only when a specific hard problem remains after focused Luna work.

Cost control comes from choosing useful scopes and the right worker effort, not from lowering the persistent lead's reasoning level. A larger file count does not itself call for a stronger worker.

```text
GPT-6 Sol MAX (persistent lead)
        │
        ├── GPT-6 Luna HIGH
        │   ├── repository exploration
        │   ├── call-site discovery
        │   ├── test discovery
        │   └── dependency analysis
        │
        ├── GPT-6 Luna MAX
        │   ├── bug investigation
        │   ├── implementation planning
        │   ├── edge cases
        │   ├── regression review
        │   └── adversarial review
        │
        └── GPT-6 Sol MAX specialist (only when justified)
            ├── architecture
            ├── concurrency
            ├── database behavior
            └── unresolved hard problems
```

The branches show available roles, not agents that must all be started. Independent scopes may run in parallel; dependent work waits for its inputs.

## Model strategy

| Responsibility | Model | Reasoning |
| --- | --- | --- |
| Persistent lead, orchestration, integration, final decisions | GPT-6 Sol | MAX |
| Repository exploration and mechanical analysis | GPT-6 Luna | HIGH |
| Bug analysis, implementation planning, edge cases, review | GPT-6 Luna | MAX |
| Difficult concurrency, database, architecture, or unresolved specialist problems | GPT-6 Sol | MAX |

### Reasoning and specialist escalation

Keep the Sol lead at MAX for every task size. For simple tasks, use fewer workers. Choose Luna HIGH for inexpensive mechanical work and Luna MAX when deeper bounded reasoning could change the outcome.

An additional Sol MAX specialist needs a concrete difficult question: for example, a subtle race, transaction invariant, architecture decision, cross-module state problem, difficult performance issue, or independent second opinion on a high-risk change. The lead decides when Luna findings are insufficient, verifies the specialist's evidence, and retains final ownership.

## Task sizing

Worker counts are planning guidance across the whole task, never quotas or simultaneous-worker targets. Use fewer when the work does not separate into valuable independent scopes.

| Size | Examples | Typical delegation |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS fix, obvious config change, tiny one-file edit | Usually no worker; optionally one GPT-6 Luna HIGH for useful discovery |
| Small | Isolated bugfix, small component or endpoint, focused test change | Roughly 1–3 GPT-6 Luna HIGH/MAX workers; Sol specialist only if unexpectedly difficult |
| Medium | Feature, CRUD flow, module refactor, API + UI + tests | Roughly 3–6 useful GPT-6 Luna HIGH/MAX scopes; optional Sol MAX specialist |
| Large | Broad refactor, repository-wide migration, new subsystem, complex regression | Roughly 6–10 useful GPT-6 Luna HIGH/MAX scopes; optional 1–2 Sol MAX specialists |

## Worker roles

The skill includes reusable roles, selected only when relevant:

| Recommended route | Roles |
| --- | --- |
| GPT-6 Luna HIGH | `repository-explorer`, `call-site-analyzer`, `test-analyzer`, `dependency-analyzer` |
| GPT-6 Luna MAX | `implementation-planner`, `bug-investigator`, `edge-case-reviewer`, `regression-reviewer`, `adversarial-reviewer` |
| GPT-6 Sol MAX, when justified | `concurrency-specialist`, `database-specialist`, `architecture-specialist` |
| GPT-6 Luna HIGH or MAX, according to difficulty | `bounded-implementer` |

Each worker gets a narrow scope, relevant context, a clear objective, allowed edits, and an expected output. Do not duplicate assignments unless an independent second opinion is intentionally useful. See [worker roles](references/roles.md) for bounded role definitions.

## Orchestration principles

The persistent lead should:

1. Understand the task, current repository state, and applicable instructions.
2. Respect existing project conventions and user requirements.
3. Decide whether delegation adds value and parallelize independent scopes.
4. Verify worker findings against the repository and resolve conflicting evidence.
5. Integrate all work into one coherent implementation and make final decisions.
6. Run relevant tests, linting, type checks, builds, and repository-specific validation.
7. Review the complete diff for regressions and unrelated changes.

Applicable instructions include `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, project documentation, and explicit user requirements. See [orchestration](references/orchestration.md) for detailed routing and integration guidance.

## Important

This skill provides orchestration instructions. It does not install models, change a subscription, enable unavailable multi-agent functionality, change the active model automatically, or bypass runtime concurrency limits. Model availability and delegation depend on the Codex environment. Explicit user instructions take precedence.

## Local validation

Check whether the repository is discovered correctly:

```powershell
bunx skills add . --list
```

Expected skill:

```text
hybrid-coding
```

## Repository structure

```text
Hybrid-Coding-Skill/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── agents/
│   └── openai.yaml
└── references/
    ├── orchestration.md
    └── roles.md
```

The repository contains a single skill, so `SKILL.md` lives directly in the repository root.

## License

Licensed under the [MIT License](LICENSE).

Copyright © 2026 Kevin Sträßler.
