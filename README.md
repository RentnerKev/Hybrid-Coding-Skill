# Hybrid Coding Skill

<!-- [![skills.sh](https://skills.sh/b/RentnerKev/Hybrid-Coding-Skill)](https://skills.sh/RentnerKev/Hybrid-Coding-Skill) -->

Cost-efficient multi-agent coding orchestration for Codex.

**GPT-6 Astra MEDIUM** acts as the persistent lead and decision-maker, while cheaper **GPT-5.6 Luna** workers handle parallel exploration, implementation analysis, testing, and review. **GPT-5.6 Terra MAX** is reserved for difficult specialist problems.

The goal is simple:

> High coding quality without wasting expensive reasoning and context tokens.

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

Select the following model in Codex:

```text
GPT-6 Astra
Reasoning: Medium
```

Then invoke the skill with your task:

```text
$hybrid-coding

Implement the requested feature, preserve the existing architecture,
run the relevant checks, and keep the change focused.
```

For most tasks, **Astra MEDIUM should remain the lead**. The skill delegates bounded work to Luna workers and only escalates when additional reasoning strength is justified.

## Why this setup?

Using the strongest model for every subtask is usually unnecessary.

Repository exploration, call-site discovery, test analysis, dependency checks, bounded implementation work, and many reviews can be handled efficiently by Luna workers.

The lead therefore keeps the important global context while workers receive smaller, focused scopes.

```text
GPT-6 Astra MEDIUM
        │
        ├── Luna HIGH
        │   ├── repository exploration
        │   ├── call-site discovery
        │   ├── test discovery
        │   └── dependency analysis
        │
        ├── Luna MAX
        │   ├── bug analysis
        │   ├── implementation planning
        │   ├── edge cases
        │   └── regression review
        │
        └── Terra MAX
            └── difficult specialist problems only
```

Worker counts are guidelines, not quotas. The skill should only spawn agents when parallelization is actually useful.

## Model strategy

| Responsibility                                            | Model         | Reasoning |
| --------------------------------------------------------- | ------------- | --------- |
| Persistent lead, orchestration, final decisions           | GPT-6 Astra   | MEDIUM    |
| Repository exploration and mechanical analysis            | GPT-5.6 Luna  | HIGH      |
| Bug analysis, implementation planning, edge cases, review | GPT-5.6 Luna  | MAX       |
| Difficult concurrency, database, or architecture problems | GPT-5.6 Terra | MAX       |

### Astra escalation

The lead should not automatically increase reasoning effort because a task is large.

Escalation is reserved for unresolved reasoning problems:

```text
MEDIUM
  ↓
HIGH
  ↓
XHIGH
  ↓
MAX
```

Prefer adding useful Luna scopes before increasing Astra reasoning effort.

## Task sizing

### Trivial

Examples:

* Rename
* Typo
* Small CSS fix
* Small configuration change
* Obvious one-file edit

Typical delegation:

```text
Astra MEDIUM
└── 0–1 Luna HIGH
```

### Small

Examples:

* Isolated bugfix
* Small component
* Small endpoint
* Focused test change

Typical delegation:

```text
Astra MEDIUM
├── 1–3 Luna HIGH
└── optional Luna MAX
```

### Medium

Examples:

* Feature implementation
* CRUD flow
* Module refactor
* API + UI + tests
* Several related files

Typical delegation:

```text
Astra MEDIUM
├── 3–6 Luna HIGH/MAX
└── Terra only if justified
```

### Large

Examples:

* Broad refactor
* Repository-wide migration
* New subsystem
* Complex regression
* Cross-module architecture change

Typical delegation:

```text
Astra MEDIUM
├── approximately 6–10 Luna HIGH/MAX
├── optional Terra MAX specialist
└── Astra escalation only when necessary
```

The numbers refer to the whole task and do not imply that all workers should run simultaneously.

## Worker roles

The skill includes reusable worker roles for common coding tasks:

* `repository-explorer`
* `call-site-analyzer`
* `test-analyzer`
* `dependency-analyzer`
* `implementation-planner`
* `bug-investigator`
* `edge-case-reviewer`
* `regression-reviewer`
* `adversarial-reviewer`
* `concurrency-specialist`
* `database-specialist`

Each worker receives:

* a clearly bounded scope
* a concrete objective
* expected output
* only the context required for its task

Workers should not duplicate each other's work unless an independent second opinion is intentionally requested.

## Orchestration principles

The lead should:

1. Understand the task and repository constraints.
2. Respect existing project instructions and conventions.
3. Determine whether delegation is useful.
4. Assign independent scopes in parallel where possible.
5. Keep expensive models focused on genuinely difficult reasoning.
6. Integrate worker findings into one coherent implementation.
7. Run relevant tests, linting, type checks, and builds.
8. Review the final diff for regressions and unnecessary changes.
9. Keep the implementation focused on the requested scope.

Existing repository instructions take precedence, including:

* `AGENTS.md`
* `CLAUDE.md`
* `CONTRIBUTING.md`
* project-specific documentation
* user-provided requirements

## Important

This skill provides orchestration instructions.

It does **not**:

* install or unlock models
* change your Codex subscription
* enable unavailable multi-agent functionality
* automatically change the active lead model
* bypass runtime concurrency limits

Model availability and agent delegation depend on the Codex environment being used.

Explicit user instructions always take precedence.

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
