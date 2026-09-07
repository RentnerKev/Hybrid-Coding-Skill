# Hybrid Coding Skill

[![skills.sh](https://skills.sh/b/RentnerKev/Hybrid-Coding-Skill)](https://skills.sh/RentnerKev/Hybrid-Coding-Skill)

A single Agent Skill for cost-efficient coding orchestration in Codex. GPT-6 Astra MEDIUM keeps task context, makes decisions, and integrates work. GPT-5.6 Luna workers handle bounded exploration, implementation, and analysis. GPT-5.6 Terra MAX is reserved for difficult specialist questions.

The aim is high coding quality with less unnecessary token and credit use. Centralizing context in one lead avoids repeating full task context across agents; focused Luna assignments move independent detail work off the lead. Actual savings depend on the task, model availability, and account pricing; worker counts are never quotas.

## Features

- Scope-aware delegation with explicit ownership and concise evidence.
- Parallel repository exploration, call-site mapping, test discovery, and analysis.
- Luna-first implementation and review, with concrete escalation criteria.
- Project conventions, existing user changes, relevant checks, and final diff review.
- A concise [skill entrypoint](SKILL.md), detailed [orchestration rules](references/orchestration.md), and reusable [worker roles](references/roles.md).

## Model strategy

Select **GPT-6 Astra / Medium** as the lead in your Codex client before starting.

| Responsibility | Model | Reasoning |
| --- | --- | --- |
| Persistent lead, decisions, integration | GPT-6 Astra | MEDIUM |
| Exploration, mechanical checks, small bounded edits | GPT-5.6 Luna | HIGH |
| Bug analysis, implementation planning, edge cases, reviews | GPT-5.6 Luna | MAX |
| Difficult concurrency, database, or architecture questions | GPT-5.6 Terra | MAX, only when justified |

Astra escalation follows MEDIUM → HIGH → XHIGH → MAX only when an important unresolved reasoning problem requires it. A larger task should usually add useful Luna scopes, not increase lead reasoning automatically.

This skill provides instructions; it does not install model configuration, unlock models, enable multi-agent tooling, or switch the active lead by itself. Routing depends on the controls and models exposed by your Codex runtime. Explicit user choices take precedence. If delegation is unavailable, the agent completes useful work locally and reports the limitation.

## Task sizes

| Size | Typical work | Luna worker guide |
| --- | --- | --- |
| Trivial | Rename, typo, CSS or config fix | 0; optional 1 HIGH |
| Small | Isolated bugfix, small component or endpoint | 1–3, mainly HIGH |
| Medium | Feature, CRUD, module refactor, API + UI + tests | Approximately 3–6 HIGH/MAX |
| Large | Broad refactor, subsystem, complex regression | Approximately 6–10 HIGH/MAX |

Counts cover the whole task, not simultaneous agents. Use fewer when work cannot be split profitably, respect runtime concurrency limits, and schedule independent scopes in waves. Terra remains at zero by default.

## Installation with Bun

With Bun installed, run:

```powershell
bunx skills add RentnerKev/Hybrid-Coding-Skill --skill hybrid-coding -g -a codex
```

Verify the global installation:

```powershell
bunx skills list -g
```

The repository root is the skill package; no nested skill directory is needed. The [skills CLI](https://github.com/vercel-labs/skills) supports root-level `SKILL.md` discovery and the installation options above.

## Use with Codex

Start with GPT-6 Astra / Medium, then invoke the skill with your task:

```text
$hybrid-coding

Implement the requested feature, preserve the existing architecture,
run the relevant checks, and keep the change focused.
```

For example, a CRUD feature might use Luna HIGH to locate project patterns and call sites, followed by Luna MAX for edge cases and regression review. The lead integrates the result and runs relevant project checks. A typo fix normally needs no workers.

Codex discovers installed skills automatically; restart the client if the skill does not appear. The optional `agents/openai.yaml` supplies UI metadata using the [official skill format](https://learn.chatgpt.com/docs/build-skills). Model routing lives in the skill instructions.

## Updates

Update this global skill:

```powershell
bunx skills update hybrid-coding -g
```

Check for available global updates:

```powershell
bunx skills check -g
```

The commands were checked against `skills` 1.5.24. Its `check` command works even though it is omitted from the top-level help.

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

To verify local discovery without installing or changing global skills:

```powershell
bunx skills add . --list
```

The expected discovered name is `hybrid-coding`.

## License

[MIT](LICENSE). Copyright © 2026 Kevin Sträßler.
