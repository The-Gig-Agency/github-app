# Rollout Plan

## Phase 1

Set up the GitHub App and webhook receiver.

Deliverables:

- GitHub App registration
- Railway service
- webhook signature verification
- installation token exchange
- basic issue trigger handling

## Phase 2

Add dispatcher and repo policy model.

Deliverables:

- repo opt-in config
- trigger validation
- idempotent job creation
- GitHub issue comments for accepted/rejected runs

## Phase 3

Connect to the orchestration layer.

Deliverables:

- job dispatch contract
- worker status polling or callback
- PR creation flow
- issue to PR linkage

## Phase 4

Operational hardening.

Deliverables:

- retries
- queue visibility
- audit trail
- per-repo policy controls
- rate limiting and abuse controls

## Open Questions

- Will orchestration live in ACP/Echelon or a separate service?
- Will workers create branches directly or through a coordinator?
- Will PR creation be automatic or gated by per-repo config?
- Do we want one worker backend or multiple worker providers?

