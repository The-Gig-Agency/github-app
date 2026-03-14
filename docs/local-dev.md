# Local Development

## Goal

Run the GitHub App locally and receive webhook events from GitHub.

## Recommended Tools

- Node.js
- npm
- `smee`

Alternative:

- ngrok

## Environment Variables

Create `.env.local` with:

```bash
PORT=3000
GITHUB_APP_ID=...
GITHUB_APP_PRIVATE_KEY=...
GITHUB_WEBHOOK_SECRET=...
ORCHESTRATOR_BASE_URL=http://localhost:4000
ORCHESTRATOR_API_KEY=local-dev-key
```

## Run Locally

```bash
npm install
npm run dev
```

## Webhook Tunneling

### Using smee

1. create smee channel
2. point GitHub App webhook URL to smee URL
3. forward smee to local server

Example:

```bash
npx smee-client --url https://smee.io/your-channel --target http://localhost:3000/api/github/webhook
```

## Local Test Cases

1. issue opened with `agent-run`
2. issue labeled with `agent-run`
3. comment `/run-agent`
4. invalid signature
5. repo not installed

## Expected Local Logs

- verified webhook
- normalized job payload
- dispatch attempted
- GitHub comment posted

