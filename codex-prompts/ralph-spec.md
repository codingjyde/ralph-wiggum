---
description: Execute the Ralph loop for a SINGLE specification.
argument-hint: "SPEC_NAME=<spec-folder-name>"
---

# Ralph Wiggum Loop: Single Spec

You are implementing a single specification for this project.

## Spec to Implement

**Spec Name**: $ARGUMENTS

Read the spec from: `specs/$ARGUMENTS/spec.md`

## Deterministic Contract

Use this strict state machine and never skip states:
1. `STATE 1: DOMAIN_REQUIRED`
2. `STATE 2: SPEC_VALIDATION`
3. `STATE 3: MATRIX_GENERATION`
4. `STATE 4: TASK_PLANNING`
5. `STATE 5: TASK_EXECUTION`
6. `STATE 6: SAFETY_GATE_VALIDATION`
7. `STATE 7: COMMIT`
8. `STATE 8: STOPPED`

Mandatory domain gate before scaffolding:
- prompt for real root domain
- prompt for required subdomains
- never use `example.com`
- stop if not confirmed

If any prerequisite is missing:
1. Insert a prerequisite task directly above the current task
2. Mark it `PREREQUISITE`
3. Commit only that prerequisite
4. Stop and enter `STATE 8`

Safety gates before commit are mandatory. Fail/stop on:
- tests not executed
- diff too large
- `console.log` outside tests
- non-epoch date persistence
- missing `organisationId` in organisation-scoped queries
- missing `{ isDeleted: false }` enforcement
- `_id` not string in mongoose models
- missing dual pagination (`cursor+size` and `page+size`)
- wrong mode import path (`./src/_common/constants/mode`)
- unconditional service startup
- wrong indentation for `.js`/`.vue` (must be 4 spaces)
- missing Quasar `public/_redirects` or non-history router
- Vue component block order violation
- constants outside `./src/_common/constants`

Run one task only. Commit once. Keep diff small.

## Completion Signal

When the spec is fully implemented and all tests pass, output:

```
<promise>DONE</promise>
```
