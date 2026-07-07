# fundamental-skills

Important skills for general purpose agentic engineering.

## What's inside

This repo aggregates general-purpose Claude Code skills:

- **`exec-plan/`** — teaches agents how to write and maintain self-contained execution plans that can be followed from design through implementation
- **`super-exec-plan/`** — standalone improved variant of exec-plan for big, important work with optional blind spot check, gap check, interview, and references

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

### `super-exec-plan`

This skill is a standalone improved variant of `exec-plan` for big, important work. It runs a collaborative planning workflow that discovers the user's unknowns through optional discovery phases before drafting the plan, and it embeds the full ExecPlan authoring guidance in its own `SKILL.md` — it has no runtime dependency on the `exec-plan` skill (or anything else).

Every discovery phase is offered via AskUserQuestion and runs only on the user's explicit yes; every finding the agent surfaces gets a user verdict (Important / False flag / Discuss) before it influences the plan. The phases map to the four quadrants of knowledge:

- known knowns → captured directly in the final ExecPlan
- known unknowns (plan ambiguities, unverbalized requirements) → the **interview** elicits them, one question at a time
- unknown knowns (recognize-it-when-you-see-it preferences) → the **references** phase extracts them from artifacts the user points at
- unknown unknowns → the **blind spot pass** (on the problem, before drafting) and the **gap check** (on the drafted plan) surface them

A session produces an artifact folder at a user-chosen destination:

```
<destination>/
  exec-plan.md            the ExecPlan; the single source of truth
  visual-explainer.html   prettified visual rendering, regenerated on every plan refinement
  mental-model.md         stub; written at implementation completion per the plan's final milestone
```

The produced plan additionally carries an "Assumptions & Open Questions" ledger with tripwires, and a mandatory final milestone requiring the implementer to write `mental-model.md` and refresh `visual-explainer.html` before declaring completion.

When to use which: invoke `/exec-plan` for smaller refactors where the requirements are already clear; invoke `/super-exec-plan` for big, important refactors or ambiguous work where the requirements, domain, or codebase hold unknowns.

## Installation

Copy any skill directory into your Claude Code skills folder:

```bash
cp -r exec-plan ~/.claude/skills/
cp -r super-exec-plan ~/.claude/skills/
```

## Usage

Invoke the skill you want in any Claude Code session:

```
/exec-plan
```

Claude will load the relevant guidance and apply it while reasoning.

## Sources

- [OpenAI Cookbook: Codex Exec Plans](https://developers.openai.com/cookbook/articles/codex_exec_plans)
