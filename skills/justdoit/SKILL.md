---
name: justdoit
description: "Default execution-planning skill for almost any non-trivial repo task. Turns a raw task, feature request, PRD, or project brief into durable execution docs: `plans.md`, `status.md`, and `test-plan.md` (or existing repo equivalents), then proposes execution before starting. Use by default unless the user clearly wants a trivial one-shot answer, pure discussion, or no planning."
---

# justdoit

Convert a task into a short execution analysis plus three durable repo files: a plan file that acts as the source of truth, a status file that acts as the live execution log, and a test plan that defines the validation and release gates.

Use the same core ideas as the PRD prompt pack: dependency-safe milestones, validation-first execution, repair-before-continue, resumability, explicit assumptions, and a clean handoff into the next execution run.

## Invocation Policy

- Treat `justdoit` as the default mode for almost any non-trivial task in a repo.
- Use it for product work, coding work, PRD decomposition, repo changes, or execution planning unless the task is obviously tiny.
- Skip it only when the user clearly wants:
  - a one-line factual answer;
  - pure brainstorming or discussion with no execution pack;
  - a trivial edit that does not benefit from planning.
- If in doubt, prefer using `justdoit`.

## Workflow

1. Determine target files.
- Prefer the repository's current convention.
- If plan/status/test-plan files already exist, update them in place.
- If the user explicitly names files or paths, follow that exactly.
- Otherwise default to `docs/plans.md`, `docs/status.md`, and `docs/test-plan.md`.

2. Normalize the input.
- Extract scope, goals, constraints, repo context, validation commands, dependencies, risks, and done definition.
- If the input is already a PRD, treat it as the source of truth.
- If the input is rough or partial, make the smallest reasonable assumptions and record them explicitly.

3. Write a short execution analysis before drafting files.
- State how the task was decomposed.
- Call out missing context, risky assumptions, and open decisions.
- Explain milestone ordering and validation strategy.
- Keep it short and operational: this is a preflight analysis, not a long essay.

4. Build the plan file.
- Make milestones small enough to finish in one focused execution loop.
- Order milestones by dependency, not by narrative.
- For each milestone include: goal, tasks, definition of done, validation commands, known risks, explicit status, and a stop-and-fix rule.
- Prefer real repo commands. If commands are missing, propose defaults and mark them as assumptions.
- Do not create milestones that end with vague "investigate more" work unless discovery is the actual task.

5. Build the status file.
- Capture current phase, done, in progress, next, decisions, assumptions, commands, blockers, audit log, and smoke/demo checks.
- The first `Next` item must point to the first unfinished plan milestone.
- Seed the file so another Codex run can resume without reconstructing context from chat history.

6. Build the test plan file.
- Capture test levels, critical fixtures, smoke coverage, negative cases, acceptance gates, and release/demo readiness checks.
- Map tests to the behavior changes implied by the plan; do not leave testing as a generic afterthought.
- If the task is docs-only or low-risk, keep the test plan compact but still explicit about what will and will not be validated.

7. Merge carefully when files already exist.
- Preserve completed items and audit history.
- Preserve useful existing test coverage notes and release gates.
- Update only affected milestones and test sections when scope changes.
- If the old structure is unusable, replace it once and record the migration in `status.md`.

8. Finish with a short execution proposal for the user.
- Do not print a full launch prompt unless the user explicitly asks for one.
- Briefly state what will start first, what execution loop will be used, and what would stop the run.
- Mention that the execution run will read the generated files first, but keep this as a concise note rather than a long pasted prompt.
- The proposal must reference the test plan when describing validation.
- Mirror the language of the user's prompt or canonical input. Do not switch to English unless the task itself was asked in English.
- Speak at the product / delivery level first. Mention technical details only if they materially affect scope, risk, or validation.
- End by asking for confirmation to start execution, unless the user already explicitly asked to execute immediately.
- If the runtime supports structured confirmation UI, use it for the execution decision. Otherwise ask one concise plain-text confirmation question.

9. Stop after generating or updating the docs and wait for confirmation, unless the user explicitly asks to execute the plan immediately.

## Writing Rules

- Keep the plan file as the source of truth. Boards and issues are mirrors, not canon.
- Use inspectable statuses: `[ ]`, `[~]`, `[x]`, or a clearly labeled status field.
- Make every validation command copy-pasteable.
- Keep assumptions in a dedicated section; never hide them inside milestone prose.
- Prefer tight bullets and tables over narrative paragraphs.
- Make milestones concrete enough that completion can be validated, not merely described.
- Keep diffs scoped when updating existing files. Do not rewrite the whole plan if one phase changed.
- The test plan must be specific to the repo and task, not a generic QA checklist.
- The execution proposal is a handoff artifact, not filler. It should reflect the actual files and milestone state you just produced.
- Keep the proposal short. The user should understand the next run in seconds, not scroll through a full prompt dump.
- Match the language already used in the request or source material.
- Do not expose the internal execution prompt. Summarize what will happen instead.
- Default to waiting for user confirmation before starting execution if the current turn was about preparing the plan pack.
- Optimize for product-level communication. Avoid reading back implementation structure unless the user asked for it.
- By default, do not paste full contents of generated files into chat when they were already written to disk.

## Missing Context Rules

- If repo validation commands are unknown, add a short `Validation assumptions` block with provisional defaults such as `lint`, `typecheck`, `test`, and `build`.
- If milestone order depends on a missing product decision, ask the user only for that decision instead of blocking the whole draft.
- If the task is too broad, split it into phases first, then fully detail phase 1 and keep later phases coarser.

## Output Format

When replying in chat after writing files, use this order:

1. Short product-level summary in 2-4 lines:
- what is now ready;
- what outcome the plan is designed to deliver;
- biggest current risk or assumption, only if it matters.
2. File paths only:
- `<plan file>`
- `<status file>`
- `<test plan file>`
3. `Ready to execute` section in 3-5 short bullets:
- what starts first;
- confirmation that the execution cycle will run;
- what validations will be used;
- what kind of blocker would stop the run.
4. Confirmation step:
- If structured confirmation UI is available, use it with concise options such as `Start now`, `Revise plan`, `Hold`.
- Otherwise ask one short plain-text confirmation question such as `Если ок, запускаю.`

Do not paste the full contents of plan, status, or test-plan into chat unless the user explicitly asks to inspect them before execution.

## Ready To Execute Pattern

Use this shape and adapt milestone names, validation commands, and language to the repo you just analyzed:

````md
## Ready to execute
- First start: <first unfinished milestone or phase>.
- I will run the execution cycle: implement -> validate -> fix -> mark done -> continue.
- Validation will use the milestone checks plus the relevant gates from the test plan.
- I will stop only for a real blocker, required secret/manual action, or irreversible approval point.

If this looks right, I can start execution next.
````

## Structured Confirmation Pattern

If the environment supports a short confirmation UI with options, prefer it over a long textual question.

Recommended options:
- `Start now` - begin execution from the first unfinished milestone.
- `Revise plan` - adjust scope, ordering, or assumptions before execution.
- `Hold` - keep the generated docs, but do not start execution yet.

If structured confirmation is not available, ask a one-line plain-text confirmation instead.

## References

- Read `references/plan-template.md` for the baseline `plans.md` structure.
- Read `references/status-template.md` for the baseline `status.md` structure.
- Read `references/test-plan-template.md` for the baseline `test-plan.md` structure.
- Reuse those templates and adapt them; do not invent a new file shape for each task unless the repo already has a stronger convention.
