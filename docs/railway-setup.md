# Railway Setup

This document covers the initial Railway setup for the GitHub App webhook service.

## Recommended Service Shape

Start with one Railway web service:

- runtime: Node.js
- responsibility: receive GitHub webhooks, validate signatures, create dispatch jobs

Optional later:

- second worker service for async job processing
- Redis or Postgres for queue/state

## App Registration Inputs

Before Railway setup, create the GitHub App under `The-Gig-Agency`.

You will need:

- `APP_ID`
- app private key PEM
- webhook secret
- app name
- installation target repos/orgs

## Railway Project Structure

Recommended:

- project: `github-app`
- service 1: `webhook-api`
- service 2 later: `worker`

## Environment Variables

### Required

- `NODE_ENV=production`
- `PORT=3000`
- `GITHUB_APP_ID=...`
- `GITHUB_APP_PRIVATE_KEY=-----BEGIN RSA PRIVATE KEY-----...`
- `GITHUB_WEBHOOK_SECRET=...`

### Orchestrator Integration

- `ORCHESTRATOR_BASE_URL=...`
- `ORCHESTRATOR_API_KEY=...`

### Optional Persistence

- `DATABASE_URL=...`
- `REDIS_URL=...`

### Optional App Behavior

- `ALLOWED_OWNERS=The-Gig-Agency,acedge123`
- `DEFAULT_BRANCH_PREFIX=codex/`
- `AGENT_REQUEST_LABEL=agent-run`
- `AGENT_RUNNING_LABEL=agent-running`
- `AGENT_BLOCKED_LABEL=agent-blocked`
- `AGENT_COMPLETE_LABEL=agent-complete`

## GitHub App Settings

### Homepage URL

Use your Railway public URL, for example:

- `https://github-app-production.up.railway.app`

### Webhook URL

Use:

- `https://github-app-production.up.railway.app/api/github/webhook`

### Webhook Secret

Set the same value in:

- GitHub App settings
- Railway `GITHUB_WEBHOOK_SECRET`

### Permissions

Repository permissions:

- Contents: Read and write
- Issues: Read and write
- Pull requests: Read and write
- Metadata: Read-only

### Subscribe To Events

- Issues
- Issue comment
- Installation
- Installation repositories

## Private Key Handling

Store the PEM exactly as a multiline Railway secret if supported.

If formatting becomes painful, store it base64-encoded instead:

- `GITHUB_APP_PRIVATE_KEY_BASE64=...`

Then decode it in app startup.

## Initial Deployment Checklist

1. Create Railway project and service
2. Connect GitHub repo `The-Gig-Agency/github-app`
3. Set required environment variables
4. Deploy app
5. Create GitHub App under `The-Gig-Agency`
6. set homepage URL
7. set webhook URL
8. set webhook secret
9. configure permissions
10. subscribe to webhook events
11. install app on target repos
12. open test issue with `agent-run`

## Health Endpoints

Recommended endpoints for the app:

- `GET /health`
- `POST /api/github/webhook`

Optional:

- `GET /api/debug/config`
- `GET /api/debug/installations`

## Recommended V1 Logging

Log:

- webhook event name
- installation ID
- owner/repo
- issue number
- trigger match result
- dispatched job ID
- outbound orchestrator status

Avoid logging:

- private key
- webhook secret
- installation tokens
- full issue bodies if they may contain secrets

## Failure Modes To Expect

### Webhook signature mismatch

Usually:

- wrong webhook secret
- body parsing changed before signature verification

### No installation token

Usually:

- app not installed on repo/org
- wrong installation ID resolution

### 403 on GitHub API

Usually:

- missing app permissions
- app installed on wrong account/org

### Trigger not firing

Usually:

- label mismatch
- event subscription missing
- repo not opted in

## Suggested Next Code Steps

1. add `docs/github-app-creation-checklist.md`
2. scaffold Node/TypeScript app
3. add webhook verification
4. add issue trigger parser
5. add orchestrator client
6. add issue comment feedback loop

