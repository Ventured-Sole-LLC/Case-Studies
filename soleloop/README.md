# SoleLoop: Serverless Change Request Platform

## The Problem
A municipal client had no formal system for managing change requests and approvals.
There was no structured way to submit a request, track its status, or route it through
a leadership approval chain with limited availability, so requests got lost in email
and paperwork.

## The Solution
A serverless AWS platform using a CQRS pattern: an append-only event log paired with a
current-state projection table, giving the client an auditable, transparent workflow
from submission to decision.

**Core services:** AWS Lambda, API Gateway, DynamoDB, Cognito, IAM
**Infrastructure:** Terraform (HCL), remote state in S3 with DynamoDB locking
**CI/CD:** GitHub Actions, secured with OIDC federation (no long-lived AWS credentials)

## Architecture Decisions
See [/architecture-decisions](./architecture-decisions) for the full set of ADRs
written during the build, covering authorization design, database schema choices,
and infrastructure decisions.