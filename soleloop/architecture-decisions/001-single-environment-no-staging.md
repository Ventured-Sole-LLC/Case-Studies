# 001: Single Environment (No Dev/Staging/Prod Split)

## Context

SoleLoop supports one client, low request volume, no dedicated ops
team on their side. Before building out deployment infrastructure,
we needed to decide whether to run a single environment or split
into separate dev, staging, and production environments.

## Decision

SoleLoop runs on a single production environment. No staging
environment was built.

## Reasoning

A staging environment exists to catch problems before they hit real
users. That matters most when deployments are frequent, or when a
bad deployment is costly. Neither applies here. Deployments are
infrequent, and if something breaks briefly, the worst case is a
delayed change request, not a safety or revenue issue.

Adding dev/staging/prod here means maintaining duplicate
infrastructure and more complex deployment logic for a problem that
doesn't exist yet. Infrastructure complexity should match the actual
requirement, not get added by default.

## Alternatives Considered

Terraform workspaces for dev/staging/prod. Rejected for now. This
makes sense for a multi-client SaaS product or a team deploying
several times a day. SoleLoop is neither, yet.

## Revisit If

- SoleLoop is adopted by more than one client at once.
- Deployment frequency increases to where testing against
  production directly becomes a real risk.
