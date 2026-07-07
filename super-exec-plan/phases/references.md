# References

You are running the references phase. Its goal is to capture what the user recognizes but cannot easily put into words — preferences, semantics, qualities — by extracting them from artifacts the user points at. Source code is the best reference; diagrams, documents, and screenshots work too.

## Ask what references exist

Ask the user what references exist: a library that implements the desired behavior, a module whose style or structure should be matched, a design or product whose feel is right, a document or spec that captures the intended semantics. References in a different language than the target are fine — you are extracting semantics and qualities, not copying code. If the user has none to offer, note that and return to the workflow.

## Read deeply and digest

Read each reference deeply — for code, the actual implementation, not just the README or API surface. Write a digest in your own words: the specific semantics, behaviors, or qualities to carry over, plus what NOT to carry over.

## Confirm with the user

Present each digest for confirmation or correction. A digest enters the plan only after the user confirms it captures what they meant; if they correct it, revise and re-confirm.

## Refine the plan

Embed each confirmed digest into exec-plan.md in your own words — never as a bare pointer; the plan must stand without access to the reference. After updating exec-plan.md, regenerate visual-explainer.html from it so the rendering does not go stale.
