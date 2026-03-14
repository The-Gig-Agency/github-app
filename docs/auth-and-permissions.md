# Auth And Permissions

## GitHub App Auth Model

The app uses:

- `APP_ID`
- app private key (`.pem`)
- webhook secret

Flow:

1. App receives webhook
2. App verifies GitHub webhook signature
3. App signs a JWT with the private key
4. App exchanges JWT for an installation token
5. App uses installation token for repo-scoped GitHub API calls

## Why GitHub App Instead Of PAT

- scoped permissions
- per-installation access
- short-lived tokens
- better auditability
- safer org-wide rollout

## Minimum V1 Permissions

Repository permissions:

- Issues: Read and write
- Pull requests: Read and write
- Contents: Read and write
- Metadata: Read-only

Organization permissions:

- none required for first pass unless using org-level admin features

Webhook subscriptions:

- Issues
- Issue comments
- Installation
- Installation repositories

## Internal Secrets

Required:

- `GITHUB_APP_ID`
- `GITHUB_APP_PRIVATE_KEY`
- `GITHUB_WEBHOOK_SECRET`

Likely also needed:

- `ORCHESTRATOR_BASE_URL`
- `ORCHESTRATOR_API_KEY`

## Security Notes

- never expose app private key to workers
- workers should receive repo access indirectly through orchestrator policy
- require explicit repo opt-in before allowing execution
- do not allow arbitrary slash commands to run destructive operations

