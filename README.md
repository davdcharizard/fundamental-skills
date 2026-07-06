# fundamental-skills

Important skills for general purpose agentic engineering.

## What's inside

This repo aggregates general-purpose Claude Code skills:

- **`exec-plan/`** — teaches agents how to write and maintain self-contained execution plans that can be followed from design through implementation

### `exec-plan`

This skill focuses on writing and using an `ExecPlan`: a living, self-contained implementation spec that a coding agent or human can follow without hidden context.

It emphasizes:

- clear user-visible outcomes and validation steps
- repository-specific orientation for novices
- living sections such as `Progress`, `Decision Log`, and `Surprises & Discoveries`
- milestone-driven delivery with proof, not just code changes

When to use `exec-plan`:

- Planning a complex feature before implementation
- Writing a spec for a substantial refactor or migration
- Turning an ambiguous request into a concrete, testable execution document
- Keeping implementation progress and design decisions in one living artifact

## Installation

Copy any skill directory into your Claude Code skills folder:

```bash
cp -r exec-plan ~/.claude/skills/
```

## Usage

Invoke the skill you want in any Claude Code session:

```
/exec-plan
```

Claude will load the relevant guidance and apply it while reasoning.

## Sources

- [OpenAI Cookbook: Codex Exec Plans](https://developers.openai.com/cookbook/articles/codex_exec_plans)
