# GitHub App

GitHub App for agent-request workflows across `The-Gig-Agency` repos.

## Purpose

This app will:

- watch GitHub Issues and issue comments for agent requests
- validate whether a repo is opted into agent execution
- dispatch work to the orchestration layer
- post progress and results back to GitHub
- optionally open pull requests tied to the originating issue

## Initial Scope

V1 is a thin dispatcher:

- GitHub App receives webhook events
- app identifies agent-run requests
- app forwards a normalized job payload to the orchestrator
- orchestrator runs Codex/Cursor/other workers
- app posts status updates back to the issue

## Ownership

- Preferred owner: `The-Gig-Agency`
- Can be installed on:
  - `The-Gig-Agency` repos
  - selected personal repos under `acedge123` if needed

## Runtime

- Recommended host: Railway
- Stack recommendation:
  - Node.js
  - TypeScript
  - Octokit
  - lightweight queue or job persistence

## Docs

- [Architecture](./docs/architecture.md)
- [Webhook Flow](./docs/webhook-flow.md)
- [Permissions](./docs/auth-and-permissions.md)
- [Rollout Plan](./docs/rollout-plan.md)

## Proposed V1 Trigger

An issue becomes actionable when either:

- it is opened with the label `agent-run`
- a maintainer comments `/run-agent`

## Proposed Repo Conventions

- issue template: `.github/ISSUE_TEMPLATE/agent-request.yml`
- per-repo opt-in config: `.github/agent-config.yml`
- labels:
  - `agent-run`
  - `agent-running`
  - `agent-blocked`
  - `agent-complete`

