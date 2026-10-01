# Hybrid Coding Skill

<!-- [![skills.sh](https://skills.sh/b/RentnerKev/Hybrid-Coding-Skill)](https://skills.sh/RentnerKev/Hybrid-Coding-Skill) -->

Cost-efficient multi-agent coding orchestration for Codex.

**GPT-6.1 Sol MAX** owns the task as the persistent lead and final decision-maker. **GPT-6 Luna HIGH** workers handle useful discovery and settled mechanical work. **GPT-6.1 Sol MEDIUM** workers handle substantive implementation, analysis, debugging, and review. Additional **GPT-6.1 Sol HIGH/MAX** specialists are reserved for concrete difficult questions or high-risk second opinions.

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
GPT-6.1 Sol
Reasoning: MAX
```

Then invoke the skill with your task:

```text
$hybrid-coding

Implement the requested feature, preserve the existing architecture,
run the relevant checks, and keep the change focused.
```

GPT-6.1 Sol MAX remains the lead throughout the task. The skill can guide delegation but cannot change the active Codex model or unlock unavailable models or agents.

## Why this setup?

GPT-6.1 Sol provides strong repository-level coding, integration, and final judgment. OpenAI recommends [GPT-6.1 Sol MEDIUM for complex technical work](https://developers.openai.com/api/docs/guides/model-selection), making it a useful starting route for substantive workers. Luna remains the inexpensive option for discovery and settled mechanical work. A Sol HIGH/MAX specialist is useful when a concrete question requires deeper reasoning or a high-risk second opinion.

Cost control comes from choosing useful scopes and worker effort while keeping the persistent lead at MAX. Consider the cost of completing the task, including lead verification, corrections, and coordination, rather than token price alone. A larger file count does not itself call for a specialist.

```text
GPT-6.1 Sol MAX (persistent lead)
        │
        ├── GPT-6 Luna HIGH
        │   ├── repository exploration
        │   ├── call-site discovery
        │   ├── test discovery
        │   ├── dependency analysis
        │   └── settled mechanical edits
        │
        ├── GPT-6.1 Sol MEDIUM
        │   ├── bounded implementation
        │   ├── bug investigation
        │   ├── implementation planning
        │   ├── edge cases
        │   ├── regression review
        │   └── adversarial review
        │
        └── GPT-6.1 Sol HIGH/MAX specialist (only when justified)
            ├── architecture
            ├── concurrency
            ├── database behavior
            └── unresolved hard problems
```

The branches show available roles, not agents that must all be started. Independent scopes may run in parallel; dependent work waits for its inputs.

## Model strategy

| Responsibility | Model | Reasoning |
| --- | --- | --- |
| Persistent lead, orchestration, integration, final decisions | GPT-6.1 Sol | MAX |
| Repository exploration and mechanical analysis | GPT-6 Luna | HIGH |
| Substantive implementation, bug analysis, planning, edge cases, review | GPT-6.1 Sol | MEDIUM |
| Difficult concurrency, database, architecture, or high-impact specialist questions | GPT-6.1 Sol | HIGH/MAX |

### Reasoning and specialist escalation

Keep the Sol lead at MAX for every task size. For simple tasks, use fewer workers. Choose Luna HIGH for discovery and mechanical work with settled contracts. Start substantive implementation, investigation, planning, and review with Sol MEDIUM.

An additional Sol HIGH/MAX specialist needs a concrete difficult question: for example, a subtle race, transaction invariant, architecture decision, cross-module state problem, difficult performance issue, or independent second opinion on a high-risk change. Choose HIGH first and MAX when the specific question warrants deeper reasoning. Route complex work directly to Sol; a preliminary Luna attempt is not required. The lead verifies the specialist's evidence and retains final ownership.

## Task sizing

Worker counts are planning guidance across the whole task, never quotas or simultaneous-worker targets. Use fewer when the work does not separate into valuable independent scopes.

| Size | Examples | Typical delegation |
| --- | --- | --- |
| Trivial | Rename, typo, small CSS fix, obvious config change, tiny one-file edit | Usually no worker; optionally one GPT-6 Luna HIGH for useful discovery |
| Small | Isolated bugfix, small component or endpoint, focused test change | Roughly 1–3 useful Luna HIGH or Sol MEDIUM scopes; HIGH/MAX only for a concrete difficult question |
| Medium | Feature, CRUD flow, module refactor, API + UI + tests | Roughly 3–6 useful Luna HIGH or Sol MEDIUM scopes; optional Sol HIGH/MAX specialist |
| Large | Broad refactor, repository-wide migration, new subsystem, complex regression | Roughly 6–10 useful Luna HIGH or Sol MEDIUM scopes; optional 1–2 Sol HIGH/MAX specialists |

## Worker roles

The skill includes reusable roles, selected only when relevant:

| Recommended route | Roles |
| --- | --- |
| GPT-6 Luna HIGH | `repository-explorer`, `call-site-analyzer`, `test-analyzer`, `dependency-analyzer` |
| GPT-6.1 Sol MEDIUM; HIGH/MAX for difficult or high-impact questions | `implementation-planner`, `bug-investigator`, `edge-case-reviewer`, `regression-reviewer`, `adversarial-reviewer` |
| GPT-6.1 Sol HIGH/MAX, when justified | `concurrency-specialist`, `database-specialist`, `architecture-specialist` |
| GPT-6 Luna HIGH for settled mechanical edits; GPT-6.1 Sol MEDIUM for substantive implementation | `bounded-implementer` |

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
