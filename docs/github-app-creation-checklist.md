# GitHub App Creation Checklist

## Where To Create It

Create the app under:

- `The-Gig-Agency`

## App Basics

- App name: `TGA Agent Dispatcher`
- Description: `Dispatches agent requests from GitHub Issues to the execution orchestrator`
- Homepage URL: Railway service URL
- Callback URL: not required for V1 unless adding user auth flows

## Webhook

- Active: yes
- Webhook URL: `https://YOUR-RAILWAY-URL/api/github/webhook`
- Webhook secret: generate and store in Railway

## Permissions

Repository permissions:

- Contents: Read and write
- Issues: Read and write
- Pull requests: Read and write
- Metadata: Read-only

## Events

Subscribe to:

- Issues
- Issue comment
- Installation
- Installation repositories

## Private Key

After creating the app:

1. generate private key
2. download `.pem`
3. store in Railway secret

## Installation

Install initially on:

- `The-Gig-Agency/github-app`

Later install on any repo that should support agent-request issues.

## First Validation

After deploy:

1. confirm `GET /health` works
2. confirm webhook deliveries succeed in GitHub App settings
3. open an issue with label `agent-run`
4. verify app comments back

