# Interview

You are running the interview. Its goal is to improve the plan by eliciting what is currently unwritten: requirements or preferences the user has not verbalized, and points where the plan is not explicit enough — anywhere an agent implementing from the plan alone could resolve an ambiguity with the wrong assumption. Often times there are design specifications that the user thinks is obvious, but the agent might not, in these cases it is important to make sure questions are asked such that these get clarified and written down.

## Evaluate the current plan

Before asking a single question, re-read the current exec-plan.md to understand what has been made clear, and what parts of the design still seem unspecified or ambiguous (and might lead to a wrong implementation different from what the user has envisioned).

## Ask one question at a time

Ask exactly one question at a time, via AskUserQuestion.

## Probe if still unclear

Watch for the user struggling to articulate an answer — hedging, describing around the thing, or saying "I'll know it when I see it". When you see it, try to ask probing questions that helps the user articulate properly what they mean so that the agent can understand.

## Refine the plan

After all questions have been answered, refine the exec-plan.md based on the new information obtained. Update the sections so an implementing agent will not make the wrong assumption against the user's desires. After updating exec-plan.md, regenerate visual-explainer.html from it (if it exists yet) so the rendering does not go stale.