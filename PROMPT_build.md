# Ralph Build Mode

Based on Geoffrey Huntley's Ralph Wiggum methodology.

---

## Phase 0: Orient

Read `.specify/memory/constitution.md` to understand project principles and constraints.

---
## Deterministic State Machine (Mandatory)

You are building a deterministic, enterprise-grade Spec-Driven Autonomous Loop.

No improvisation.
No stack drift.
No speculative scaffolding.
No implicit assumptions.
Stop until it is done.

Execution states (strictly linear):
1. `STATE 1: DOMAIN_REQUIRED`
2. `STATE 2: SPEC_VALIDATION`
3. `STATE 3: MATRIX_GENERATION`
4. `STATE 4: TASK_PLANNING`
5. `STATE 5: TASK_EXECUTION`
6. `STATE 6: SAFETY_GATE_VALIDATION`
7. `STATE 7: COMMIT`
8. `STATE 8: STOPPED`

If any state fails:
- Insert prerequisite task directly above current task, mark it `PREREQUISITE`
- Commit only prerequisite work
- Stop execution and enter `STATE 8: STOPPED`
- Do not continue original task

## Mandatory Domain Input

Before any scaffolding:
- Prompt for real root domain (never `example.com`)
- Prompt for required subdomains
- Require explicit confirmation

If missing:
- Insert prerequisite
- Commit it
- Stop until it is done

## Mandatory Rules

- Do NOT restructure this Ralph runner repository. It may contain scripts/, templates/, codex-prompts/, skills/, etc.
- In the TARGET SOLUTION REPOSITORY (the app being built), the repository root must contain only `./docs` and `./code`
- Specs live in `./docs/specs` with: version, objectives, constraints, contracts, events, acceptance tests, Service Matrix, Client Matrix, Technology Matrix
- Persisted dates must be epoch milliseconds via `Date.now()`
- Folder-based modules only; 4 spaces in `.js` and `.vue`
- Shared code in `./src/_common`; constants in `./src/_common/constants` as `Object.freeze` enums
- Mode enum must exist at `./src/_common/constants/mode/index.js`
- Runtime services must start conditionally by MODE (`ALL`, `WEB`, `REALTIME`, `WORKER`)
- Backend lock: Express/CommonJS/async-await, socket.io, FreeSWITCH, agenda.js, MongoDB/Mongoose, winston JSON logging, no `console.log` outside tests
- Mongoose models: string `_id`, `_id` auto disabled, epoch `createdAt`/`updatedAt`, `isDeleted`, `scopeMode`
- Organisation-scoped queries must include `organisationId`
- Soft delete enforcement required: `{ isDeleted: false }`
- Pagination required on list endpoints: cursor+size and page+size, default 25, max 100, stable sort, cursor preferred
- Client lock: Quasar + Vue3 + Vuex + Options API + history mode + Netlify `_redirects`; no React/Pinia/Composition API/TypeScript unless spec explicitly requires
- Vue component order must match mandated order exactly

## Safety Gate (Before Commit)

Abort commit and enter `STATE 8: STOPPED` if any fail:
- Diff too large
- `console.log` outside tests
- Spec changed without version bump
- Tests not executed
- Secrets in logs
- Missing organisationId for organisation scope
- Missing soft delete enforcement
- `_id` not string
- Non-epoch date persistence
- Missing pagination dual support
- Mode not imported from `_common/constants`
- Unconditional service startup
- Wrong indentation
- Missing Quasar `_redirects`
- Router not history mode
- Vue component order incorrect
- Constant enum outside `_common/constants`

## Execution Contract

1. `STATE 1` prompt for domain
2. `STATE 2` validate spec
3. `STATE 3` generate matrices
4. `STATE 4` plan tasks
5. `STATE 5` execute one task
6. `STATE 6` run safety gate validation
7. `STATE 7` commit
8. `STATE 8` stopped on failure

One task.
One commit.
Keep diffs small.
Never continue past uncertainty.

## Completion Signal

Only output `<promise>DONE</promise>` when one task is completed, safety gates pass, commit succeeded, and no unresolved prerequisite exists.
