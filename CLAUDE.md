
# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Task Breakdown Before Implementation

**Before coding, convert the user's request into clear sections and actionable subtasks.**

For every non-trivial request:

* First, identify the main goal.
* If the request contains multiple different areas of work, split it into separate sections.
* Each section must contain a checklist of concrete subtasks.
* If the request is simple and only has one area of work, still create a small checklist for that single task.
* Add a short success or verification condition for each section.
* Do not start implementation until the task breakdown is written.

Use this format:

```md
## Task Plan

### Section 1: [Name]
Goal: [What this section is supposed to achieve]
Verify: [How to confirm this section is complete]

- [ ] Subtask 1
- [ ] Subtask 2
- [ ] Subtask 3

### Section 2: [Name]
Goal: [What this section is supposed to achieve]
Verify: [How to confirm this section is complete]

- [ ] Subtask 1
- [ ] Subtask 2
```

During implementation:

* Work through the subtasks one by one.
* After completing each subtask, mark it as done with `[x]`.
* If a subtask is started but not finished, mark it as `[~]` and briefly explain what remains.
* Do not skip subtasks silently.
* If a subtask becomes unnecessary, explain why before removing or ignoring it.
* If a new required subtask appears during implementation, add it to the checklist before doing it.

Example:

```md
## Task Plan

### Backend Changes
Goal: Add server-side validation for the requested input.
Verify: Invalid inputs are rejected and related tests pass.

- [x] Find the relevant API endpoint
- [x] Add input validation
- [ ] Add or update tests
- [ ] Run the test suite

### Frontend Changes
Goal: Update the UI so users can see validation errors.
Verify: The error message appears correctly in the related form.

- [x] Locate the related component
- [ ] Update the UI text
- [ ] Verify responsive behavior
```

The goal is to make progress visible, prevent hidden assumptions, and ensure every implementation step is traceable to the user's request.



## 6. Persistent Resume / Progress Tracking

**Always keep the implementation state visible and resumable.**

For long, multi-step, or risky tasks, maintain a temporary progress file in the repository:

```txt
.claude-task-progress.md
```

Use this file when:

* The request has multiple sections or subtasks.
* The work may take more than one coding session.
* The implementation touches multiple files.
* The task may be interrupted.
* The user may later say `continue`, `resume`, or ask to keep working from the previous state.

The file should contain:

* User request summary
* Assumptions
* Task sections
* Subtask checklist
* Completed work
* In-progress work
* Pending work
* Verification results
* Known issues or blockers
* Exact resume point

Required structure:

```md
# Claude Task Progress

## User Request
[Short summary of what the user asked]

## Assumptions
- [Assumption 1]
- [Assumption 2]

## Task Progress

### Section: [Name]
Goal: [Goal]
Verify: [Verification method]

- [x] Completed task
- [~] In-progress task — remaining: [remaining work]
- [ ] Pending task

## Completed Work
- [What was implemented]
- [Files changed, if useful]

## Verification Results
- [x] Test/build/check completed: [command/result]
- [ ] Pending verification: [what still needs to be checked]

## Known Issues / Blockers
- [Issue or blocker, if any]

## Resume Point
Continue from:
- Section: [section name]
- Subtask: [subtask name]
- Next action: [exact next step]
```

During implementation:

* Update `.claude-task-progress.md` after each meaningful completed subtask.
* Do not mark a subtask as complete unless it has been implemented and verified.
* If a new required task appears, add it to the progress file before doing it.
* If a task becomes unnecessary, keep it visible and explain why it was skipped.
* Do not silently skip, merge, or delete tasks.

When resuming after interruption:

1. Read `.claude-task-progress.md` first if it exists.
2. Compare the progress file with the current repository state.
3. Re-check completed `[x]` tasks briefly to avoid duplicating work.
4. Identify the first `[~]` or `[ ]` task.
5. Continue from the `Resume Point`.
6. Do not restart the whole task unless the previous state is missing, inconsistent, or clearly broken.
7. If the previous state is unclear, explain what is unclear, inspect the codebase, then decide the next step.

Cleanup rule:

* Keep `.claude-task-progress.md` while the task is active.
* When the full task is complete and verified, either delete it or update it with a final completion summary.
* Do not leave stale progress files that describe already-finished work as still pending.

The goal is to make the work resumable after interruptions and to prevent the model from losing track of what was already done.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

@project.md
