---
description: Execute the Ralph loop to process ALL specifications in specs/ folder sequentially.
argument-hint: "[--max-iterations=<number>]"
---

# Ralph Wiggum Loop: Process All Specs

You are implementing all specifications in the `specs/` folder for this project.

## Deterministic Contract

For each spec, execute this strict state machine in order:
1. `STATE 1: DOMAIN_REQUIRED`
2. `STATE 2: SPEC_VALIDATION`
3. `STATE 3: MATRIX_GENERATION`
4. `STATE 4: TASK_PLANNING`
5. `STATE 5: TASK_EXECUTION`
6. `STATE 6: SAFETY_GATE_VALIDATION`
7. `STATE 7: COMMIT`
8. `STATE 8: STOPPED`

Before scaffolding:
- prompt for real root domain and required subdomains
- do not use `example.com`
- do not continue without explicit confirmation

If any missing prerequisite is discovered:
1. Insert a task directly above current task
2. Mark it `PREREQUISITE`
3. Commit only prerequisite work
4. Stop execution and enter `STATE 8`

Safety gates are required before every commit. Abort on:
- tests not executed
- diff too large
- `console.log` outside tests
- non-epoch date persistence
- organisation-scoped query without `organisationId`
- missing soft-delete query enforcement
- non-string `_id` in mongoose models
- missing dual pagination strategy
- mode import not from `./src/_common/constants/mode`
- unconditional startup of all services
- bad `.js`/`.vue` indentation (must be 4 spaces)
- missing Quasar `_redirects` or router not history mode
- wrong Vue component option order
- constants defined outside `./src/_common/constants`

One task per iteration.
One commit per task.
Small diffs only.
Stop on uncertainty.

## Completion Signal

When ALL specs are complete and verified, output:

```
<promise>ALL_DONE</promise>
```

## Arguments

$ARGUMENTS
