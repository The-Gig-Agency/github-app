# Webhook Flow

## Events to Subscribe To

V1:

- `issues`
- `issue_comment`
- `installation`
- `installation_repositories`

Optional later:

- `pull_request`
- `pull_request_review`
- `check_run`

## Trigger Rules

### Issue Opened

Trigger only if:

- issue has label `agent-run`

### Issue Labeled

Trigger only if:

- added label is `agent-run`

### Issue Comment

Trigger only if comment body starts with:

- `/run-agent`
- `/agent plan`
- `/agent retry`

## Normalized Job Payload

```json
{
  "installationId": 123,
  "owner": "The-Gig-Agency",
  "repo": "some-repo",
  "issueNumber": 42,
  "trigger": "issue_comment",
  "command": "/run-agent",
  "title": "Implement onboarding flow",
  "body": "Full issue body here",
  "labels": ["agent-run", "priority-high"]
}
```

## Immediate App Responses

When triggered, app should:

1. add `agent-running`
2. comment that the request was accepted
3. persist job ID

Example comment:

> Agent run accepted. Creating execution job and preparing repo context.

## Completion Responses

Success:

- remove `agent-running`
- add `agent-complete`
- comment summary
- link PR if created

Failure:

- remove `agent-running`
- add `agent-blocked`
- comment blocker summary

