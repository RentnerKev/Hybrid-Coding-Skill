# Hybrid Coding Skill

Selective multi-agent coding for Codex and Claude Code. The main agent owns the task, delegates work with a clear scope, verifies the results, and integrates the changes.

## Codex

Install the [Codex skill](codex/hybrid-coding/SKILL.md) globally with Bun. Run the same command to update it, including when upgrading from v1.3.0 or earlier.

```powershell
bunx skills add https://github.com/RentnerKev/Hybrid-Coding-Skill/tree/main/codex/hybrid-coding --skill hybrid-coding -g -a codex
```

Select **GPT-6.2 Sol** with reasoning **MAX**, then invoke `$hybrid-coding` with your task. GPT-6 Luna HIGH handles discovery and settled mechanical work. GPT-6.2 Sol MEDIUM handles scoped implementation, debugging, and review. Use Sol HIGH/MAX for difficult questions and GPT-6.2 Astra HIGH/MAX for specialist work or a consequential second opinion. [Codex routing and worker guidance](codex/hybrid-coding/references/orchestration.md)

## Claude Code

Install the [Claude Code plugin](claude) globally. The plugin loads the skill and its four native subagents together. Use Claude Code v2.1.321 or later for Fable 5.2 support. [Claude Code model requirements](https://code.claude.com/docs/en/model-config#work-with-fable)

```powershell
claude plugin marketplace add RentnerKev/Hybrid-Coding-Skill
claude plugin install hybrid-coding@rentnerkev-hybrid-coding --scope user
```

Start the lead session in your project with **Opus 5.5 Extra High**.

```powershell
claude --model claude-opus-5-5 --effort xhigh
```

Invoke `/hybrid-coding:hybrid-coding-claude` with your task. Haiku 5.5 handles discovery, Sonnet 5.5 handles scoped implementation and review, Opus 5.5 handles deeper analysis, and Fable 5.2 handles the hardest specialist questions. Opus can take over when Fable is unavailable. [Claude Code routing](claude/skills/hybrid-coding-claude/SKILL.md)

To update an existing installation, refresh the marketplace and then update the plugin.

```powershell
claude plugin marketplace update rentnerkev-hybrid-coding
claude plugin update hybrid-coding@rentnerkev-hybrid-coding --scope user
```

## Repository structure

| Path | Contents |
| --- | --- |
| [`codex/hybrid-coding/`](codex/hybrid-coding) | Codex skill, OpenAI metadata, and worker references |
| [`claude/`](claude) | Claude Code plugin, skill, and subagent definitions |
| [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) | Repository-level marketplace entry that points Claude Code to `claude/` |

## License

Licensed under the [MIT License](LICENSE).

Copyright © 2026 Kevin Sträßler.
