# Blind Spots

You are running the blind spots phase. Its goal is to present the user with unknowns about the problem they have not considered — things they did not know to ask about — get each one resolved or at least understood, and refine the exec plan with what comes back.

Calibrate to the user. A user new to the domain needs domain teaching: concepts where they do not yet know what "good" looks like, standard pitfalls. A domain expert needs codebase-specific surprises instead. Aim the pass at where this user's map is most likely to diverge from the territory.

## Research first

Before presenting anything, research, using the codebase as the primary source and the web where it helps.

Try to consider:
- Domain knowledge the user may lack: concepts, invariants, or standard practices in this problem's domain.
- Codebase history and conventions: existing utilities that already do part of the job, past attempts (dead code, reverted commits, TODO trails), and established patterns the plan should follow or consciously break. Check git history when informative.
- Hidden coupling: code, configuration, tests, or downstream consumers affected by this change but never mentioned. Search for callers, importers, and fixtures — not just the files the user named.
- Operational concerns: migration of existing data or state, rollout and backward compatibility, performance under real load, failure handling.
- Scope alternatives: signs the problem might be better solved a different way — a smaller change that captures most of the value, an existing tool that already does this, or evidence the stated problem is a symptom of a different one.

On a repeat pass, do not re-raise blind spots already discovered. Focus areas the user may still be unaware of based on the state of the plan.

## Present in small batches

Present blind spots in small batches (roughly two to four at a time), not all at once. Each gets a short name; a one-paragraph, plain-language explanation of why it might matter for this task specifically; and pointers to evidence (file paths with line references, commit hashes, or links).

## Resolve each blind spot with the user

For each blind spot, collect the user's verdict via AskUserQuestion: "Important — address in plan", "False flag — dismiss", "Discuss first". On "Discuss first", talk it through until the user lands on an action plan for the blind spot, or thinks it should be dismissed. After resolution refine the plan with the new information.

## Refine the plan

Given the discussion on the blind spots update the exec-plan.md. Make refinements or additions to sections where natural e.g. typically a Decision Log entry plus updates to whichever sections it changes (milestones, plan of work, validation, ...). One that is understood but cannot be resolved yet goes into Assumptions & Open Questions. After updating exec-plan.md, regenerate visual-explainer.html from it (if it exists yet) so the rendering does not go stale.