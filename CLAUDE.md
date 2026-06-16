
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

Before coding, write the full task plan and present it to the user. Do not start implementation until plan is written and fully listed.

- Split unrelated areas of work into separate sections, each with its own subtasks.
- Present ALL sections and ALL subtasks first, then implement one by one.

```md
## Task Plan

### Section: [Name]
Goal: [What to achieve]
Verify: [How to confirm done]

- [ ] Subtask 1
- [ ] Subtask 2
```

Rules: `[x]` = done, `[~]` = in progress (explain what remains), `[ ]` = pending. Never skip silently. Add new subtasks before doing them. Explain before removing.



## 6. Persistent Resume / Progress Tracking

For multi-step, multi-file, or interruptible tasks, maintain `.claude-task-progress.md`:

```md
# Claude Task Progress

## User Request
[Summary]

## Assumptions
- [Assumption]

## Task Progress

### Section: [Name]
Goal: [Goal]
Verify: [Verification method]

- [x] Completed task
- [~] In-progress — remaining: [what's left]
- [ ] Pending task

## Verification Results
- [x] [command/result]
- [ ] [pending check]

## Known Issues / Blockers
- [Issue]

## Resume Point
- Section: [name] / Subtask: [name] / Next: [exact action]
```

Update after each subtask. On resume: read file first, verify `[x]` items, continue from Resume Point. Delete file when task is fully complete.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

@project.md
