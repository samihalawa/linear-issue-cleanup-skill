---
name: linear-issue-cleanup-skill
description: "Clean up a Linear backlog by closing issues already addressed in code or production state, rescheduling and clarifying issues that still matter, and then systematically implementing the remaining validated work. Use when the user wants Linear kept accurate and wants execution to continue issue by issue instead of stopping at backlog triage."
---

# Linear Issue Cleanup Skill

Use this skill when the user wants a real Linear cleanup pass, not just a status summary.

The job is:

1. reconcile the actual Linear backlog against the current repo, runtime state, comments, and recent commits
2. close or archive issues that are already addressed or obsolete
3. update, clarify, and schedule issues that are still valid but not yet done
4. then systematically execute the remaining issues in dependency order
5. keep Linear accurate while the implementation proceeds

This skill is execution-first. Do not stop after triage if the user asked to proceed through the remaining work.

## Hard Rules

- Work in the real local repo and the real Linear workspace.
- Do not trust issue titles, statuses, or stale comments without verification.
- Verify each issue against code, configs, recent commits, and any relevant runtime or deployment evidence before changing its state.
- Close issues only when they are truly addressed, obsolete, duplicate, or superseded.
- When closing an issue, leave a short factual note if helpful so the cleanup is auditable.
- Do not mark unresolved work as done just because part of it exists.
- If an issue is still valid but underspecified, rewrite it into an actionable state before scheduling it.
- Scheduling means setting the right status and, when the workspace uses them, assigning owner, priority, cycle, milestone, due date, or project so the issue can actually be worked.
- After the cleanup pass, continue into execution for the remaining validated issues unless the user explicitly asked only for triage.
- Keep implementation aligned with the current codebase; do not force old issue narratives onto a changed architecture.

## Required First Pass

Before editing product code, complete this reconciliation pass:

1. identify the relevant team, project, assignee, cycle, or query scope in Linear
2. list the active issues in scope
3. inspect current repo state, recent commits, and local uncommitted work
4. map issues to real code areas, workflows, and any already-shipped changes
5. classify each issue as one of:
   - addressed
   - obsolete
   - duplicate
   - valid-needs-update
   - valid-ready-to-execute
6. update Linear so the backlog reflects reality
7. only then start implementation on the remaining issues

## Workflow

### 1. Build The Real Scope

- Determine the intended Linear scope from the user request:
  - team
  - project
  - assignee
  - cycle
  - label
  - or a specific issue set
- Use the real Linear tools to fetch the issue list, issue details, comments, statuses, and project metadata.
- Inspect the local repo before making issue decisions:
  - `tree -I node_modules -L 2`
  - `git status --short --branch`
  - `git log --oneline --decorate -30`
- If relevant, inspect deployments, logs, or runtime behavior so issue status is based on evidence.

### 2. Reconcile Every Issue Against Reality

For each issue in scope:

- read the full issue description, comments, labels, status, relations, and assignee
- inspect the referenced files and workflows in the current codebase
- review recent commits for matching work
- determine whether the issue is:
  - already addressed in the current code or shipped state
  - obsolete because the product or architecture changed
  - duplicated by another issue
  - still valid but missing clarity or schedule
  - still valid and ready for implementation now

Do not make status decisions from issue text alone.

### 3. Clean Up Linear First

For issues that are already addressed:

- move them to the appropriate completed or canceled state
- add a concise comment when helpful describing the evidence

For duplicates or obsolete issues:

- close or cancel them deliberately
- link the superseding issue when one exists

For valid but stale issues:

- rewrite the title or description if needed so the work is specific
- update priority and status
- assign the correct owner if obvious from the repo context
- place the issue into the correct cycle, milestone, or due-date schedule when that workflow exists
- remove ambiguity before execution begins

### 4. Build The Execution Queue

After cleanup, order the remaining valid issues by:

1. blockers and dependencies
2. production impact
3. user-facing severity
4. implementation leverage
5. verification cost

Prefer finishing one coherent slice at a time rather than scattering partial work across many issues.

### 5. Execute The Remaining Issues Systematically

For each issue selected for execution:

- inspect all related files and existing logic first
- implement the full fix or feature directly in the current codebase
- verify the affected workflow with the fastest reliable real checks
- commit the finished work as a coherent change
- push the branch the user expects for normal project workflow
- update the Linear issue status, comments, and any scheduling metadata to reflect the real result

If solving one issue also resolves related issues, update all of them in the same pass.

### 6. Keep Linear And Code In Sync

As work proceeds:

- move in-progress work to the correct active state
- close issues only after verification
- comment briefly with what changed when that helps future cleanup
- adjust downstream issue schedules if blockers were removed or priorities changed

### 7. Finish Cleanly

At the end of the run, report:

- which issues were closed as already addressed
- which issues were closed as obsolete or duplicate
- which issues were updated or rescheduled
- which issues were implemented
- which valid issues remain and in what execution order

The cleanup is only successful if Linear now matches reality and execution on the remaining work has already started or completed.
