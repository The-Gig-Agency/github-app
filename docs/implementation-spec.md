# Implementation Spec

## Stack

Use:

- Node.js
- TypeScript
- Express
- Octokit

Optional later:

- Prisma
- Postgres
- Redis

## Initial File Layout

```text
src/
  server.ts
  routes/
    health.ts
    github-webhook.ts
  github/
    app.ts
    verifyWebhook.ts
    installation.ts
    comments.ts
    labels.ts
  dispatch/
    trigger.ts
    normalize.ts
    orchestrator.ts
  config/
    env.ts
  types/
    github.ts
```

## Required Endpoints

### `GET /health`

Response:

```json
{ "ok": true }
```

### `POST /api/github/webhook`

Responsibilities:

- read raw body
- verify GitHub signature
- parse event type
- process supported events
- return `200` quickly

## Supported GitHub Events

- `issues`
- `issue_comment`
- `installation`
- `installation_repositories`

## Trigger Rules

### Issue Opened

Dispatch only if:

- action is `opened`
- label list contains `agent-run`

### Issue Labeled

Dispatch only if:

- action is `labeled`
- added label is `agent-run`

### Issue Comment

Dispatch only if:

- issue is not pull request-backed
- comment starts with one of:
  - `/run-agent`
  - `/agent plan`
  - `/agent retry`

## Immediate GitHub Side Effects

On accepted trigger:

- add label `agent-running`
- add acknowledgement comment

On rejected trigger:

- no-op or comment only if useful

## Acknowledgement Comment

Use:

```text
Agent request accepted. Preparing repo context and creating an execution job.
```

## Orchestrator Dispatch

Send normalized request to the orchestrator.

See [orchestrator-contract.md](./orchestrator-contract.md).

## Initial Persistence Strategy

V1 can be stateless except for optional idempotency cache.

Acceptable first pass:

- no database
- dedupe by GitHub delivery ID in memory
- upgrade later to Postgres/Redis

## Logging Requirements

Every webhook handling path should log:

- event name
- action
- delivery ID
- installation ID
- owner/repo
- issue number if present
- accepted vs ignored
- orchestrator status code

## Error Handling

If orchestrator dispatch fails:

- leave `agent-running` off if dispatch never succeeded
- add comment:

```text
Agent request could not be dispatched. Please retry or inspect service logs.
```

## Branch Naming

When a worker creates branches, default prefix:

- `codex/`

## V1 Non-Goals

- merge PRs automatically
- execute destructive commands directly from GitHub comments
- support multiple orchestrators in first pass

