# Orchestrator Contract

## Purpose

Define the HTTP contract between the GitHub App and the execution orchestrator.

## Endpoint

Recommended:

- `POST {ORCHESTRATOR_BASE_URL}/api/agent-jobs`

## Auth

Header:

- `Authorization: Bearer {ORCHESTRATOR_API_KEY}`

## Request Body

```json
{
  "source": "github",
  "installationId": 123456,
  "deliveryId": "uuid-from-github",
  "owner": "The-Gig-Agency",
  "repo": "github-app",
  "issueNumber": 42,
  "trigger": "issue_comment",
  "command": "/run-agent",
  "title": "[Agent Request] Build webhook service",
  "body": "Issue body text",
  "labels": ["agent-run"],
  "repositoryUrl": "https://github.com/The-Gig-Agency/github-app",
  "defaultBranch": "master"
}
```

## Success Response

```json
{
  "ok": true,
  "jobId": "job_123",
  "status": "queued"
}
```

## Failure Response

```json
{
  "ok": false,
  "error": "reason"
}
```

## Optional Callback Contract

If the orchestrator pushes updates back later, recommended endpoint:

- `POST /api/orchestrator/callback`

Payload:

```json
{
  "jobId": "job_123",
  "status": "running",
  "issueNumber": 42,
  "owner": "The-Gig-Agency",
  "repo": "github-app",
  "message": "Worker has started"
}
```

Possible statuses:

- `queued`
- `running`
- `blocked`
- `completed`
- `failed`

## PR Completion Payload

```json
{
  "jobId": "job_123",
  "status": "completed",
  "owner": "The-Gig-Agency",
  "repo": "github-app",
  "issueNumber": 42,
  "prNumber": 17,
  "prUrl": "https://github.com/The-Gig-Agency/github-app/pull/17",
  "summary": "Implemented webhook verification and dispatch."
}
```

