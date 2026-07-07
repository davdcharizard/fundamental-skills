---
name: super-exec-plan
description: Guide to writing and using a super execution plan from design to implementation. Use for large or ambiguous work where the requirements, domain, or codebase hold many unknowns and need comprehensive planning, deliberation, and refinement to develop a strong approach.
---

# Super ExecPlan

You are running a collaborative planning workflow. This skill produces an execution plan ("ExecPlan"),
a design document that a coding agent can follow to deliver a working feature or system change.
The ExecPlan is optionally enriched by discovery phases that surface the user's unknowns for
making refinements to the plan.

The framing: the map, a representation of the work to be done, consists of the prompts and skills and context provided by the user.
The territory is where the work needs to happen, the codebase, the real world, and its actual constraints.
Your job in each phase is to shrink the gap between the map and the territory prior to moving forward with implementation.

## The collaboration contract (non-negotiable)

1. This workflow is Socratic and cooperative. You and
   the user build and iterate the plan together through back-and-forth until
   the user is satisfied with the state of the plan.
2. Every optional phase begins with an AskUserQuestion asking whether to
   run it.
3. Findings you surface (blind spots, gap-check findings) should be checked
   by the user. You may be wrong about what matters, so treat
   the user as the ultimate judge. Record every verdict, including dismissals and the
   user's reason, so false flags are not rediscovered later.
4. During discovery phases, never resolve an ambiguity silently. Either
   ask, or record it as an explicit assumption in the plan.

## Workflow

Phases run in this order on the first pass when the exec plan is getting created
for the first time. Optional phases require the user's yes via AskUserQuestion at the moment the phase
would begin. Before running a phase, Read its phase file from this
skill's phases/ directory and follow it.

Once an exec plan exists, any phase can be re-run on request (see "Iterating and resuming"). 

The plan evolves on disk. exec-plan.md is created at START and
updated as new information is produced via new findings,
new decisions, new requirements.

The document serves as the plan's working state: at any moment the user can
read the current draft plan and give grounded feedback to refine it further.

1. START. The user usually provides initial design specifications
   and context when invoking the skill; read that carefully
   first. Settle the destination next: if the user named the
   artifact folder's location, use it; otherwise ask now via
   AskUserQuestion, proposing sensible candidates (the user can
   type a custom path under "Other"). If the destination already
   holds an exec-plan.md, do not redraft — resume per "Iterating
   and resuming". Otherwise draft an initial exec plan from what
   you have, following the "Authoring the ExecPlan" guidance below,
   and write it to exec-plan.md inside the folder. A rough first
   draft is fine; record everything uncertain in its Assumptions &
   Open Questions section.

2. BLIND SPOT PASS (gated) — read phases/blind-spot.md.
   Targets unknown unknowns in the problem: presents blind spots
   the user has not considered, gets each resolved or at least
   understood with the user, and folds the outcomes into
   exec-plan.md across its sections before moving on.

3. INTERVIEW (gated) — read phases/interview.md.
   Targets what is currently unwritten: requirements the user has
   not verbalized, and points the plan leaves ambiguous that an
   implementing agent could resolve with a wrong assumption. One
   question at a time, architecture-changing questions first. Fold
   each answer into exec-plan.md as it lands.

4. REFERENCES (gated) — read phases/references.md.
   Targets unknown knowns: things the user recognizes but cannot
   easily describe. Produces confirmed reference digests. Embed
   each confirmed digest into exec-plan.md before moving on.

5. REFINE THE EXECPLAN (always). Revise exec-plan.md into a complete
   plan by following the "Authoring the ExecPlan" guidance below to
   the letter. Everything gathered so far should already be in the
   document — blind spot resolutions, interview answers, reference
   digests (embedded in your own words — no external pointers) — so
   this step's job is coherence and completeness:
   make each input traceable in the plan's Decision Log, resolve
   what can now be resolved, and ensure the plan contains the
   section described under "Assumptions & Open Questions" below and
   the mandatory final milestone described under "Output artifacts".
   Generate visual-explainer.html and the mental-model.md stub
   alongside the markdown.

6. GAP CHECK (gated; offer it only after the draft exists) — read
   phases/gap-check.md. Targets unknown unknowns in the plan itself:
   what gaps has the plan not covered, what ambiguities or
   uncertainties remain? Confirmed findings revise the plan; accepted
   residual risks become ledger entries.

7. FINAL REVIEW (always). Summarize the plan, the surviving
   assumptions and tripwires, and the recorded dismissals. Iterate
   with the user until they say the plan is done — including
   re-running any phase they ask for. On every refinement, update
   exec-plan.md first and regenerate visual-explainer.html from it;
   before finishing, confirm all three artifact files exist and the
   HTML matches the final markdown.

## Iterating and resuming

One pass (steps 1–7) is often enough, but planning takes as many
rounds as the user wants. Once exec-plan.md exists, in any state:

- The user may request any phase again at any time, in any order —
  another gap check, another blind spot pass, an interview on a
  specific area ("interview me on X"). Read the phase file and run
  it, scoped to the area the user named or to what changed since
  the last pass. Do not re-raise findings already recorded in the
  plan, including dismissed ones.
- Each re-run folds its results into exec-plan.md exactly as on the
  first pass, and revisions regenerate visual-explainer.html once
  it exists.
- If the skill is invoked and a draft exec-plan.md already exists
  at the destination (e.g. a later session continuing earlier
  planning), read the plan first — it is the workflow's state —
  skip START's drafting, and continue from whatever phase or
  refinement the user asks for.

## Output artifacts

The workflow's output is a folder, not a single file. The user
usually names the destination in the invocation ("write it at
/path/my-feature-plan"); if they did not, ask via AskUserQuestion at
START, before the initial draft is written. exec-plan.md exists from
START onward and evolves with every step; visual-explainer.html and
the mental-model.md stub are generated at the refine step. The
folder always ends up containing exactly these three fixed names:

    <folder>/
      exec-plan.md            the ExecPlan; the single source of truth
      visual-explainer.html   prettified visual rendering of the plan
      mental-model.md         stub now; written at implementation completion

visual-explainer.html is a single self-contained HTML page that
re-presents the ExecPlan using what HTML offers beyond markdown —
layout, color, diagrams, timelines, collapsible sections — wherever
visuals genuinely aid explanation and communication of the plan. It
is a rendering, never a fork: it must not contain plan content
absent from the markdown. Whenever exec-plan.md is refined — during
this workflow or later during implementation — regenerate it.

mental-model.md at planning time is only a stub stating its purpose.
Its real content, written by the implementing agent at completion,
is the high-level design reasoning and architecture of the system AS
ACTUALLY BUILT — the final mental model after all deviations and
discoveries are settled, not the initial intended design — the why
behind what exists, which the codebase alone cannot convey.

Every ExecPlan this workflow produces MUST therefore include a final
milestone, gating completion, that requires the implementer to
(a) write mental-model.md as described above and (b) regenerate
visual-explainer.html from the finished exec-plan.md.

## Assumptions & Open Questions (the unknowns ledger)

Every ExecPlan this workflow produces must contain a section titled
"Assumptions & Open Questions". Each entry has three parts:

- Assumption or open question: stated plainly.
- Basis: why we currently believe it / why it is still open.
- Tripwire: a concrete observable condition during implementation
  that, if hit, means the assumption is wrong — and the instruction to
  STOP and revisit the plan with the user rather than improvise.

During and after implementation, unknowns are captured by the
ExecPlan's mandatory living sections — Surprises & Discoveries at
every stopping point, the Decision Log for course changes, and
Outcomes & Retrospective at completion — with the tripwires above
marking when a surprise means STOP and re-plan with the user.

# Authoring the ExecPlan

This part describes the requirements for the execution plan ("ExecPlan") that this workflow writes to `exec-plan.md` — a design document that a coding agent can follow to deliver a working feature or system change. Assume the reader is a beginner to the repository: they have only the current working tree and the single ExecPlan file you provide. There is no memory of prior plans and no external context.

A note on authority: during the super-exec-plan planning workflow, the collaboration contract above governs — ambiguities are resolved with the user, not autonomously. The autonomous-execution instructions in this part ("resolve ambiguities autonomously", "do not prompt the user for next steps") apply only to the later implementation phase, and even then they are bounded by the tripwires in Assumptions & Open Questions, which mandate stopping to re-plan with the user when hit.

## How to author and use an ExecPlan

### Authoring ExecPlan

When authoring an executable specification (ExecPlan), follow the instructions below _to the letter_. Be thorough in reading and re-reading the instructions to produce an accurate specification. When creating a spec, start from the skeleton and flesh it out as you do your research.

### Using ExecPlan

When using an ExecPlan, treat it as a living document.

When implementing an executable specification (ExecPlan), do not prompt the user for "next steps"; simply proceed to the next milestone. Keep all sections up to date, add or split entries in the list at every stopping point to affirmatively state the progress made and next steps. Resolve ambiguities autonomously, and commit frequently.

When discussing an executable specification (ExecPlan), record decisions in a log in the spec for posterity; it should be unambiguously clear why any change to the specification was made. ExecPlans are living documents, and it should always be possible to restart from _only_ the ExecPlan and no other work.

When researching a design with challenging requirements or significant unknowns, use milestones to implement proof of concepts, "toy implementations", etc., that allow validating whether the user's proposal is feasible. Read the source code of libraries by finding or acquiring them, research deeply, and include prototypes to guide a fuller implementation.

## Requirements

NON-NEGOTIABLE REQUIREMENTS:

* Every ExecPlan must be fully self-contained. Self-contained means that in its current form it contains all knowledge and instructions needed for a novice to succeed.
* Every ExecPlan is a living document. Contributors are required to revise it as progress is made, as discoveries occur, and as design decisions are finalized. Each revision must remain fully self-contained.
* Every ExecPlan must enable a complete novice to implement the feature end-to-end without prior knowledge of this repo.
* Every ExecPlan must produce a demonstrably working behavior, not merely code changes to "meet a definition".
* Every ExecPlan must define every term of art in plain language or do not use it.

Purpose and intent come first. Begin by explaining, in a few sentences, why the work matters from a user's perspective: what someone can do after this change that they could not do before, and how to see it working. Then guide the reader through the exact steps to achieve that outcome, including what to edit, what to run, and what they should observe.

The agent executing your plan can list files, read files, search, run the project, and run tests. It does not know any prior context and cannot infer what you meant from earlier milestones. Repeat any assumption you rely on. Do not point to external blogs or docs; if knowledge is required, embed it in the plan itself in your own words. If an ExecPlan builds upon a prior ExecPlan and that file is checked in, incorporate it by reference. If it is not, you must include all relevant context from that plan.

## Formatting

Format and envelope are simple and strict. Each ExecPlan must be one single fenced code block labeled as `md` that begins and ends with triple backticks. Do not nest additional triple-backtick code fences inside; when you need to show commands, transcripts, diffs, or code, present them as indented blocks within that single fence. Use indentation for clarity rather than code fences inside an ExecPlan to avoid prematurely closing the ExecPlan's code fence. Use two newlines after every heading, use # and ## and so on, and correct syntax for ordered and unordered lists.

When writing an ExecPlan to a Markdown (.md) file where the content of the file *is only* the single ExecPlan, you should omit the triple backticks.

Write in plain prose. Prefer sentences over lists. Avoid checklists, tables, and long enumerations unless brevity would obscure meaning. Checklists are permitted only in the `Progress` section, where they are mandatory. Narrative sections must remain prose-first.

## Guidelines

Self-containment and plain language are paramount. If you introduce a phrase that is not ordinary English ("daemon", "middleware", "RPC gateway", "filter graph"), define it immediately and remind the reader how it manifests in this repository (for example, by naming the files or commands where it appears). Do not say "as defined previously" or "according to the architecture doc." Include the needed explanation here, even if you repeat yourself.

Avoid common failure modes. Do not rely on undefined jargon. Do not describe "the letter of a feature" so narrowly that the resulting code compiles but does nothing meaningful. Do not outsource key decisions to the reader. When ambiguity exists, resolve it in the plan itself and explain why you chose that path. Err on the side of over-explaining user-visible effects and under-specifying incidental implementation details.

Anchor the plan with observable outcomes. State what the user can do after implementation, the commands to run, and the outputs they should see. Acceptance should be phrased as behavior a human can verify ("after starting the server, navigating to [http://localhost:8080/health](http://localhost:8080/health) returns HTTP 200 with body OK") rather than internal attributes ("added a HealthCheck struct"). If a change is internal, explain how its impact can still be demonstrated (for example, by running tests that fail before and pass after, and by showing a scenario that uses the new behavior).

Specify repository context explicitly. Name files with full repository-relative paths, name functions and modules precisely, and describe where new files should be created. If touching multiple areas, include a short orientation paragraph that explains how those parts fit together so a novice can navigate confidently. When running commands, show the working directory and exact command line. When outcomes depend on environment, state the assumptions and provide alternatives when reasonable.

Be idempotent and safe. Write the steps so they can be run multiple times without causing damage or drift. If a step can fail halfway, include how to retry or adapt. If a migration or destructive operation is necessary, spell out backups or safe fallbacks. Prefer additive, testable changes that can be validated as you go.

Validation is not optional. Include instructions to run tests, to start the system if applicable, and to observe it doing something useful. Describe comprehensive testing for any new features or capabilities. Include expected outputs and error messages so a novice can tell success from failure. Where possible, show how to prove that the change is effective beyond compilation (for example, through a small end-to-end scenario, a CLI invocation, or an HTTP request/response transcript). State the exact test commands appropriate to the project's toolchain and how to interpret their results.

Capture evidence. When your steps produce terminal output, short diffs, or logs, include them inside the single fenced block as indented examples. Keep them concise and focused on what proves success. If you need to include a patch, prefer file-scoped diffs or small excerpts that a reader can recreate by following your instructions rather than pasting large blobs.

## Milestones

Milestones are narrative, not bureaucracy. If you break the work into milestones, introduce each with a brief paragraph that describes the scope, what will exist at the end of the milestone that did not exist before, the commands to run, and the acceptance you expect to observe. Keep it readable as a story: goal, work, result, proof. Progress and milestones are distinct: milestones tell the story, progress tracks granular work. Both must exist. Never abbreviate a milestone merely for the sake of brevity, do not leave out details that could be crucial to a future implementation.

Each milestone must be independently verifiable and incrementally implement the overall goal of the execution plan.

Every plan's final milestone, gating completion, must additionally require the implementer to (a) write `mental-model.md` — the high-level design reasoning and architecture of the system as actually built, after all deviations and discoveries are settled, per "Output artifacts" above — and (b) regenerate `visual-explainer.html` from the finished `exec-plan.md` so the rendering matches the final plan. No plan may be declared complete before both are done.

## Living plans and design decisions

* ExecPlans are living documents. As you make key design decisions, update the plan to record both the decision and the thinking behind it. Record all decisions in the `Decision Log` section.
* ExecPlans must contain and maintain a `Progress` section, a `Surprises & Discoveries` section, a `Decision Log`, and an `Outcomes & Retrospective` section. These are not optional.
* When you discover optimizer behavior, performance tradeoffs, unexpected bugs, or inverse/unapply semantics that shaped your approach, capture those observations in the `Surprises & Discoveries` section with short evidence snippets (test output is ideal).
* If you change course mid-implementation, document why in the `Decision Log` and reflect the implications in `Progress`. Plans are guides for the next contributor as much as checklists for you.
* At completion of a major task or the full plan, write an `Outcomes & Retrospective` entry summarizing what was achieved, what remains, and lessons learned.

# Prototyping milestones and parallel implementations

It is acceptable—-and often encouraged—-to include explicit prototyping milestones when they de-risk a larger change. Examples: adding a low-level operator to a dependency to validate feasibility, or exploring two composition orders while measuring optimizer effects. Keep prototypes additive and testable. Clearly label the scope as "prototyping"; describe how to run and observe results; and state the criteria for promoting or discarding the prototype.

Prefer additive code changes followed by subtractions that keep tests passing. Parallel implementations (e.g., keeping an adapter alongside an older path during migration) are fine when they reduce risk or enable tests to continue passing during a large migration. Describe how to validate both paths and how to retire one safely with tests. When working with multiple new libraries or feature areas, consider creating spikes that evaluate the feasibility of these features _independently_ of one another, proving that the external library performs as expected and implements the features we need in isolation.

## Skeleton of a Good ExecPlan

    # <Short, action-oriented description>

    This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

    If PLANS.md file is checked into the repo, reference the path to that file here from the repository root and note that this document must be maintained in accordance with PLANS.md.

    ## Purpose / Big Picture

    Explain in a few sentences what someone gains after this change and how they can see it working. State the user-visible behavior you will enable.

    ## Progress

    Use a list with checkboxes to summarize granular steps. Every stopping point must be documented here, even if it requires splitting a partially completed task into two ("done" vs. "remaining"). This section must always reflect the actual current state of the work.

    - [x] (2025-10-01 13:00Z) Example completed step.
    - [ ] Example incomplete step.
    - [ ] Example partially completed step (completed: X; remaining: Y).

    Use timestamps to measure rates of progress.

    ## Surprises & Discoveries

    Document unexpected behaviors, bugs, optimizations, or insights discovered during implementation. Provide concise evidence.

    - Observation: …
      Evidence: …

    ## Decision Log

    Record every decision made while working on the plan in the format:

    - Decision: …
      Rationale: …
      Date/Author: …

    ## Outcomes & Retrospective

    Summarize outcomes, gaps, and lessons learned at major milestones or at completion. Compare the result against the original purpose.

    ## Assumptions & Open Questions

    Carry every unknown that survives planning into this ledger. Each entry has three parts:

    - Assumption or open question: stated plainly.
      Basis: why we currently believe it, or why it is still open.
      Tripwire: a concrete observable condition during implementation that, if hit, means the assumption is wrong — STOP and revisit the plan with the user rather than improvise.

    ## Context and Orientation

    Describe the current state relevant to this task as if the reader knows nothing. Name the key files and modules by full path. Define any non-obvious term you will use. Do not refer to prior plans.

    ## Plan of Work

    Describe, in prose, the sequence of edits and additions. For each edit, name the file and location (function, module) and what to insert or change. Keep it concrete and minimal.

    ## Concrete Steps

    State the exact commands to run and where to run them (working directory). When a command generates output, show a short expected transcript so the reader can compare. This section must be updated as work proceeds.

    ## Validation and Acceptance

    Describe how to start or exercise the system and what to observe. Phrase acceptance as behavior, with specific inputs and outputs. If tests are involved, say "run <project's test command> and expect <N> passed; the new test <name> fails before the change and passes after>".

    ## Idempotence and Recovery

    If steps can be repeated safely, say so. If a step is risky, provide a safe retry or rollback path. Keep the environment clean after completion.

    ## Artifacts and Notes

    Include the most important transcripts, diffs, or snippets as indented examples. Keep them concise and focused on what proves success.

    ## Interfaces and Dependencies

    Be prescriptive. Name the libraries, modules, and services to use and why. Specify the types, traits/interfaces, and function signatures that must exist at the end of the milestone. Prefer stable names and paths such as `crate::module::function` or `package.submodule.Interface`. E.g.:

    In crates/foo/planner.rs, define:

        pub trait Planner {
            fn plan(&self, observed: &Observed) -> Vec<Action>;
        }

If you follow the guidance above, a single, stateless agent -- or a human novice -- can read your ExecPlan from top to bottom and produce a working, observable result. That is the bar: SELF-CONTAINED, SELF-SUFFICIENT, NOVICE-GUIDING, OUTCOME-FOCUSED.

When you revise a plan, you must ensure your changes are comprehensively reflected across all sections, including the living document sections, and you must write a note at the bottom of the plan describing the change and the reason why. ExecPlans must describe not just the what but the why for almost everything.
