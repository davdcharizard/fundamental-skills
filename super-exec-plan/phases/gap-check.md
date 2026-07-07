# Gap Check

You are running the gap check. Its goal is to interrogate the drafted plan for gaps, ambiguities, and uncertainties before implementation begins. It runs only after a draft exec-plan.md exists.

## Re-read the draft cold

Re-read exec-plan.md from top to bottom as a skeptical reviewer who was not part of the planning conversation and has only the document. The governing questions: what gaps has this plan not covered? What ambiguities or uncertainties remain?

Try to consider:
- Coverage gaps: affected code, tests, configurations, or behaviors the plan never mentions.
- Ambiguities: steps a novice implementer could reasonably interpret two different ways.
- Unstated assumptions: environment details, library behaviors, or invariants of the existing code the plan silently relies on.
- Premortem: imagine the plan was executed and the result failed or disappointed the user; narrate concretely why.
- Validation blind spots: ways the plan's acceptance steps could all pass while the behavior is still wrong.
- Intent drift: places where the plan achieves the letter of the request but misses what the user actually wants.

On a repeat pass, do not re-raise findings already discussed; focus on sections that changed since the last pass or areas the user names.

## Present and resolve findings

Present findings in small batches (roughly two to four at a time). Each names the gap or ambiguity, the section of the plan it concerns, and why it matters. For each, collect the user's verdict via AskUserQuestion: "Important — address in plan", "False flag — dismiss", "Discuss first". On "Discuss first", talk it through until the user lands on an action for the finding or dismisses it.

## Refine the plan

Given the resolutions, update exec-plan.md. Make refinements or additions to sections where natural — typically the affected sections plus a Decision Log entry. An uncertainty that cannot be resolved before implementation goes into Assumptions & Open Questions. After updating exec-plan.md, regenerate visual-explainer.html from it so the rendering does not go stale.
