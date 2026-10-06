---
name: exec-spec
description: Guide to writing a self-contained product feature specification that can be handed over to the infra team for implementation. Use to help facilitate handoff between product team's feature prototype and infra team development.
---

# Execution Spec

This document describes the requirements for an execution spec ("ExecSpec"), a product feature spec that can evolve over time during product development and prototyping, or written when the feature has reached its final shape. The ExecSpec describes the core design choices, constraints, and requirements for defining a product feature. An infra engineer should be able to review and follow the execution spec document independently to realize the feature without misalignment with the product team or designer.

An "ExecSpec" prioritizes high level specifications (like user stories, core requirements, important implementation decisions including how tradeoffs should be managed) and DOES NOT contain low level implementation details UNLESS it fits into a "core constraint" or "requirement" for the feature behavior or mechanism. For instance, if a particular approach to implementation was done implicitly without documentation where it could have been easily done differently without violating core feature shape, requirements or constraints, assume this is NOT a core decision and to leave it out of the ExecSpec.

## How to author an ExecSpec

When authoring an executable specification (ExecSpec), follow the instructions below _to the letter_. Be thorough in reading and re-reading the instructions to produce an accurate specification. When creating a spec, start from the skeleton and flesh it out as you do your research.

Use all the context available from product development, including but not limited to:
* Meeting notes from Feishu discussing the feature
* Personal notes on the feature
* Codex / Claude Code session(s) working on the feature (important design requirements and decisions can be found in the user prompts and feedback to the agent)

If new context or information is provided, the pre-existing specification must be updated and refined to reflect it.

## Requirements

NON-NEGOTIABLE REQUIREMENTS:

* Every ExecSpec must be fully self-contained. Self-contained means that in its current form it contains all important feature requirements, constraints, and decisions (extracted from the available context) for the infra team to be able to realize the feature without product misalignment or ambiguity as to how things should behave at a high level.
* Every ExecPlan is a living document. Contributors are required to revise it as new information becomes available, as new design decisions are made. Each revision must remain fully self-contained.
* Every ExecPlan must enable a infra engineer to implement the feature end-to-end without needing to consult the product designer.
* Every ExecPlan must define every term of art in plain language or do not use it.

## Formatting

Format and envelope are simple and strict. Each ExecPlan must be one single fenced code block labeled as `md` that begins and ends with triple backticks. Do not nest additional triple-backtick code fences inside; when you need to show commands, transcripts, diffs, or code, present them as indented blocks within that single fence. Use indentation for clarity rather than code fences inside an ExecSpec to avoid prematurely closing the ExecSpec's code fence. Use two newlines after every heading, use # and ## and so on, and correct syntax for ordered and unordered lists.

When writing an ExecSpec to a Markdown (.md) file where the content of the file *is only* the single ExecSpec, you should omit the triple backticks.

Write in plain prose. Prefer sentences over lists. Avoid checklists, tables, and long enumerations unless brevity would obscure meaning. Checklists are permitted only in the `Progress` section, where they are mandatory. Narrative sections must remain prose-first.

## Evolving Document

Treat an ExecSpec as constraining and defining the form of the current product feature. It is not an implementation plan. Revise it whenever new information or context materially change the feature requirements. After every revision, reconcile the entire document so that its active sections describe one coherent current specification without any contradictory statements. Stale information must be removed comprehensively during revisions.

When a choice changes, remove the superseded choice from the active requirements or decisions and preserve it under `Decision History` with the reason it changed.

## Conflict Reconciliation

Resolve conflicting decisions or evidence by preferring the most current and authoritative source. When final product code conflicts with earlier notes, plans, or session context, treat the code as the current behavior and the prior notes stale, because the current code form is more likely to reflect the latest decision (especially if the decision was not explicitly recorded).

## Explicit vs. Implicit Choices

High level requirements are typically more important in the ExecSpec than low level requirements, but both should be included. The real boundary for what should be included and excluded from the ExecSpec is whether something is **explicitly designed or implemented one way**. Therefore even low-level detail should be included if it is explicitly made as a core constraint on the product feature behavior or mechanism.

Separate explicit decisions from implicit ones. Prioritize and make sure explicit decisions make it into the ExecSpec while keeping implicit ones (especially those that can be done in many alternative ways) without user involvement out.

When referencing a prototype (e.g. codebase), how it is currently implemented does not mean every part is a core requirement or decision for that feature. Assume that everything not constrained by an explicit requirement, implementation decision, schema, or contract remains flexible and may be subject to change, hence should not be part of the ExecSpec.

If a schema, contract, or low-level choice appears important but no source shows that it was explicitly chosen for reliance, this must be surfaced to the user as an ambiguity to resolve.

Keep the ExecSpec concise enough for a PM to review and edit. Prefer a small number of precise requirements and representative examples over exhaustive detail.

## Skeleton of a Good ExecSpec

    # <Short, concise title describing the feature>

    This ExecSpec is a living document. Keep it aligned with the latest product behavior and decisions, and preserve superseded or rejected choices under `Decision History`.

    ## Purpose / Big Picture

    Explain in a few sentences why the feature matters, the user or product problem it solves, and the outcome it should create. Keep this section high level and easy for a PM to verify.

    ## Feature Requirements

    State the current non-negotiable product requirements and constraints. Focus on user-visible behavior, core mechanism requirements, and the principles for resolving meaningful tradeoffs. Distinguish required outcomes from illustrative implementation ideas. Anything not constrained here or in an explicit later section remains flexible.

    Organize related requirements together and explain the motivation when it is necessary to interpret the requirement correctly. Do not include project tasks, implementation sequence, or incidental details of the current code.

    ## User Stories / Examples

    Give a small set of end-to-end stories that collectively cover the main feature behavior. Then add only the edge cases that reveal non-obvious constraints or meaningfully stress the robustness of the feature.

    For each story, identify the user's situation and intent, the action or event, and the observable result. Use concrete examples to disambiguate requirements, not to prescribe an implementation or repeat every requirement in narrative form.

    ## Context & Orientation

    List the source documents, meeting notes, session records, plans, code locations, or other artifacts worth consulting. For each source, provide its link or repository-relative path and one concise description of the specific content it is useful for. Do not summarize the whole source.

    References in this section supplement the ExecSpec; they must not substitute for important requirements, constraints, or decisions. Carry all normative product information into the relevant self-contained section above or below.

    ## Implementation Decisions

    Record only implementation decisions that were made explicitly and that constrain how the feature must be realized. These decisions usually emerge during prototyping or implementation and are more specific than the initial feature requirements, but they still represent intentional product or mechanism choices rather than incidental code structure.

    Organize decisions by product or technical area from high level to low level. For each decision, state the chosen approach, why it was chosen, and the constraint it places on future implementations. Omit choices that can be changed without affecting the specified behavior. Do not promote an inferred choice into this section without signoff.

    ## Decision History

    Preserve decisions that were previously adopted and later changed or removed, along with why they changed. Also record alternatives that were explicitly considered but never adopted and why they were rejected or deferred.

    Make it unmistakable that these entries are historical and not current requirements. Do not use this section as a chronological activity log; include only history that helps reviewers understand the present specification or avoid repeating a rejected direction.

    ## Schemas & Contracts

    Define the schemas and contracts that any implementation of the feature must preserve. Include a contract only when the team explicitly shaped it. State clearly why the contract was designed this way (e.g. prioritized simplicity in the contract design so LLMs can better use it), and the intended consumers i.e. what relies on it.

    Provide enough exact detail to prevent incompatible implementations, using concise field definitions, signatures, or examples where needed. Do not freeze an interface merely because it exists in the current code. If reliance is only implicit or the reason for preserving it is unclear, surface to the user for confirmation.

If you follow the guidance above an infra engineer or an agent -- can read your ExecSpec from top to bottom and using its own desired approach / available resources / knowledge, produce a working product feature that aligns with all the specified product constraints and requirements. That is the bar: SELF-CONTAINED, SELF-SUFFICIENT, TOP-TO-BOTTOM, CONSTRAINT AND REQUIREMENT FOCUSED.

When you revise a plan, you must ensure your changes are comprehensively reflected across all sections. ExecSpecs must describe not just the what but the why for almost everything.
