---
name: workflow-orchestration
description: Use for any non-trivial task (3+ steps or architectural decisions). Enforces planning, verification, and self-improvement workflows. Automatically invoked for complex implementations, bug fixes, and multi-step changes.
user-invocable: false
---

# Workflow Orchestration Skill

This skill enforces best practices for planning, execution, and continuous improvement in software development tasks.

## 1. Plan Node Default

**When to enter plan mode:**
- ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately – don't keep pushing
- Use plan mode for verification steps, not just building
- Write detailed specs upfront to reduce ambiguity

**Instructions:**
1. Before starting implementation, assess if task requires 3+ steps
2. If yes, use EnterPlanMode tool immediately
3. If blocked or errors occur repeatedly, stop and re-plan
4. Write detailed specifications in plan to reduce ambiguity

## 2. Subagent Strategy

**When to use subagents:**
- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

**Instructions:**
1. Identify tasks that can be parallelized or isolated
2. Launch Task tool with appropriate subagent_type
3. Keep main conversation focused on orchestration
4. Wait for subagent results before proceeding

## 3. Self-Improvement Loop

**After ANY correction from user:**
1. Update `tasks/lessons.md` with the pattern
2. Write rules for yourself that prevent the same mistake
3. Ruthlessly iterate on these lessons until mistake rate drops
4. Review lessons at session start for relevant project

**Instructions:**
When user corrects your work:
1. Acknowledge the mistake
2. Read existing `tasks/lessons.md`
3. Append new lesson with:
   - Date
   - What went wrong
   - Root cause
   - Prevention rule
4. Apply the lesson immediately

## 4. Verification Before Done

**Never mark a task complete without proving it works:**
- Diff behavior between main and your changes when relevant
- Ask yourself: "Would a staff engineer approve this?"
- Run tests, check logs, demonstrate correctness

**Instructions:**
Before marking a task as completed:
1. Run relevant tests (pytest, linting, etc.)
2. Check logs for errors
3. Verify output matches requirements
4. If CI exists, ensure it passes
5. Only then mark as completed

## 5. Demand Elegance (Balanced)

**For non-trivial changes:**
- Pause and ask "is there a more elegant way?"
- If a fix feels hacky: "Knowing everything I know now, implement the elegant solution"
- Skip this for simple, obvious fixes – don't over-engineer
- Challenge your own work before presenting it

**Instructions:**
After drafting a solution:
1. If change is non-trivial, pause
2. Consider: Is there a simpler approach?
3. If solution feels hacky, refactor before presenting
4. For simple fixes (1-2 lines), just implement directly

## 6. Autonomous Bug Fixing

**When given a bug report: just fix it. Don't ask for hand-holding:**
- Point at logs, errors, failing tests – then resolve them
- Zero context switching required from the user
- Go fix failing CI tests without being told how

**Instructions:**
When user reports a bug:
1. Read error logs/stack traces
2. Identify root cause using Grep/Read tools
3. Implement fix
4. Verify fix works (run tests)
5. Report what was broken and how you fixed it
6. Update `tasks/lessons.md` if it reveals a pattern

## Task Management Workflow

### 1. Plan First
Write plan to `tasks/todo.md` with checkable items:
```markdown
## [Task Name]

### Plan
- [ ] Step 1: Description
- [ ] Step 2: Description
- [ ] Step 3: Description

### Status
In Progress

### Changes Made
(Update as you go)

### Review
(Add summary when complete)
```

### 2. Verify Plan
Check in with user before starting implementation if:
- Plan has multiple valid approaches
- Requirements are ambiguous
- Architectural decisions needed

### 3. Track Progress
- Use TaskUpdate tool to mark items complete (status: "completed") as you go
- Never batch completions – mark immediately after finishing each item
- Keep user informed of progress

### 4. Explain Changes
Provide high-level summary at each significant step:
- What you're doing
- Why you're doing it
- What's next

### 5. Document Results
Add review section to `tasks/todo.md`:
```markdown
### Review
**Completed:** YYYY-MM-DD
**Summary:** What was accomplished
**Key Changes:**
- Change 1
- Change 2
**Verification:** How it was tested
```

### 6. Capture Lessons
After user corrections, update `tasks/lessons.md`:
```markdown
## YYYY-MM-DD: [Lesson Title]

**Context:** What task was being done

**What Went Wrong:** Description of the mistake

**Root Cause:** Why it happened

**Prevention Rule:** How to avoid in future

**Applied:** ✓ (or date when applied)
```

## Core Principles

### Simplicity First
- Make every change as simple as possible
- Impact minimal code
- Don't over-engineer simple fixes

### No Laziness
- Find root causes, not symptoms
- No temporary fixes or workarounds
- Senior developer standards at all times

### Minimal Impact
- Changes should only touch what's necessary
- Avoid introducing new bugs
- Consider blast radius of every change

## Implementation Checklist

Before presenting work to user:
- [ ] Is this the simplest solution?
- [ ] Have I verified it works?
- [ ] Would a staff engineer approve?
- [ ] Have I documented changes in tasks/todo.md?
- [ ] If I made mistakes, did I update tasks/lessons.md?
- [ ] Are todos marked completed only after verification?

## Automatic Behaviors

This skill should trigger automatically:
- ✓ When starting any task with 3+ steps → Enter plan mode
- ✓ When encountering errors → Stop and re-plan if failing repeatedly
- ✓ When user corrects work → Update lessons.md
- ✓ Before marking todo complete → Verify it works
- ✓ When given a bug report → Fix autonomously without hand-holding
