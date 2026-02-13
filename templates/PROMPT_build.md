# Ralph Build Mode (Deterministic)

You are building a deterministic, enterprise-grade Spec-Driven Autonomous Loop.

No improvisation.
No stack drift.
No speculative scaffolding.
No implicit assumptions.
Stop until it is done.

## State Machine Model (Mandatory)

Execution states:

1. `STATE 1: DOMAIN_REQUIRED`
2. `STATE 2: SPEC_VALIDATION`
3. `STATE 3: MATRIX_GENERATION`
4. `STATE 4: TASK_PLANNING`
5. `STATE 5: TASK_EXECUTION`
6. `STATE 6: SAFETY_GATE_VALIDATION`
7. `STATE 7: COMMIT`
8. `STATE 8: STOPPED`

Transitions are strictly linear.

If any state fails:
- Insert prerequisite task.
- Commit only prerequisite task.
- Stop all execution.
- Enter `STATE 8: STOPPED`.

Never continue past uncertainty.

## Mandatory Domain Input

Before scaffolding:
- Prompt for real root domain.
- Prompt for required subdomains.
- Do not use `example.com`.
- Do not proceed without explicit confirmation.

If missing:
- Insert prerequisite.
- Commit it.
- Stop until it is done.

## Prerequisite Enforcement (Non-Negotiable)

If a missing prerequisite is discovered:
1. Insert a new task directly above the current task.
2. Mark it as `PREREQUISITE`.
3. Commit only that prerequisite.
4. Stop all execution.
5. Do not resume original task until prerequisite is complete.

## Root Structure Rule

Target product repository root must contain only:
- `./docs`
- `./code`

All specs and documentation -> `./docs`
All implementation -> `./code`

If violated:
- Insert prerequisite.
- Commit it.
- Stop until it is done.

## Date Storage Rule

All persisted dates must be epoch milliseconds (`Date.now()`).

Do not store:
- ISO strings
- Date objects
- Formatted date strings

Violation fails safety gate.

## File and Formatting Rules

- Folder-based modules only (`./contact/index.js`, not `./contact.js`)
- 4 spaces indentation for `.js` and `.vue`
- No 2-space indentation
- No mixed tabs/spaces

Violation fails safety gate.

## Common Code and Constants

Shared code path: `./src/_common`

Constants path: `./src/_common/constants`

Constants rules:
- Object.freeze enums
- Folder-based modules with `index.js`
- Never redefined
- Never mutated

Mode enum must exist at `./src/_common/constants/mode/index.js`:

```js
const Mode = Object.freeze({
    ALL: 0,
    WEB: 1,
    REALTIME: 2,
    WORKER: 3
});

module.exports = {
    Mode: Mode
};
```

If missing:
- Insert prerequisite.
- Commit it.
- Stop until it is done.

## Runtime Mode System

Server must not start all services unconditionally.

`MODE` is read from environment. Default: `Mode.ALL`.

Service startup matrix:
- `ALL`: WEB + REALTIME + WORKER
- `WEB`: Express only
- `REALTIME`: socket.io only
- `WORKER`: agenda only

Mode import path: `./src/_common/constants/mode`

Unconditional startup fails safety gate.

## Backend Technology Lock

- Web: Express.js + CommonJS + async/await
- Realtime: socket.io
- Telephony: FreeSWITCH only
- Jobs: agenda.js
- Database: MongoDB + Mongoose
- Logging: winston only, structured JSON
- No `console.log` outside tests
- Process manager: pm2 only if required

Deviation:
- Insert prerequisite.
- Commit it.
- Stop until it is done.

## Logging Standard

- winston only
- JSON output
- required fields: timestamp, level, service, environment
- no secrets in logs
- centralized logger module only
- no inline logger configuration

Violation fails safety gate.

## Mongoose Model Rules

Every model must:
1. Use string `_id`
2. Disable auto `_id` and set `_id` default to `new ObjectId().toString()`
3. Store `createdAt` and `updatedAt` as epoch milliseconds
4. Include `isDeleted`
5. Declare `scopeMode`: `organisation` or `host`

If `scopeMode = organisation`, all queries must include `organisationId`.

Missing enforcement fails safety gate.

## Soft Delete

All applicable queries must enforce:

`{ isDeleted: false }`

Missing enforcement fails safety gate.

## Pagination (Mandatory)

All list endpoints must support:
1. `cursor + size`
2. `page + size`

Rules:
- default size = 25
- max size = 100
- stable sorting required
- cursor mode wins if both provided

Missing support fails safety gate.

## Client Stack Rules

Web:
- Quasar
- Vue 3
- Vuex
- Options API
- History mode router
- Netlify deployment

Netlify SPA fallback required at `public/_redirects`:

`/* /index.html 200`

Mobile: NativeScript only, if explicitly required by spec.

Do not use:
- React
- Composition API
- Pinia
- TypeScript unless explicitly required

## Vue Component Order (Strict)

```js
export default {
    name: '',
    data: function () { return {}; },
    computed: {},
    watch: {},
    created: function () {},
    mounted: function () {},
    methods: {},
    components: {},
    props: {}
};
```

Incorrect order fails safety gate.

## Spec System

Specs must live in `./docs/specs`.

Every spec must include:
- version
- objectives
- constraints
- contracts
- events
- acceptance tests
- Service Matrix
- Client Matrix
- Technology Matrix

No implementation before spec validation (`STATE 2`).

## Safety Gates (Before Commit)

Abort commit if any fail:
- Diff too large
- `console.log` outside tests
- Spec changed without version bump
- Tests not executed
- Secrets in logs
- organisation-scoped query missing `organisationId`
- `isDeleted` enforcement missing
- `_id` not string
- Dates not epoch milliseconds
- Pagination missing both strategies
- Mode not imported from `_common/constants`
- Services start unconditionally
- Incorrect indentation
- Missing Quasar `_redirects`
- Router not history mode
- Vue component order incorrect
- Constant enum defined outside `_common/constants`

If any safety gate fails: abort commit and enter `STATE 8: STOPPED`.

## Execution Flow

1. `STATE 1` -> Prompt for domain
2. `STATE 2` -> Validate spec
3. `STATE 3` -> Generate matrices
4. `STATE 4` -> Plan tasks
5. `STATE 5` -> Execute one task
6. `STATE 6` -> Run safety gates
7. `STATE 7` -> Commit
8. `STATE 8` -> STOPPED on failure

One task.
One commit.
Small diff.
No continuation past uncertainty.

## Completion Signal

Only output `<promise>DONE</promise>` when:
- One task has been completed
- Safety gate passed
- Commit completed
- No unresolved prerequisite exists
