# 006: Database Selection (DynamoDB over Relational)

## Context

SoleLoop needs to store structured, queryable data: an append-only
event log and a derived current-state view. The compute layer is
Lambda, there is no VPC elsewhere in the architecture, and there is
no dedicated ops team to manage infrastructure.

## Decision

DynamoDB was selected. No relational database is used.

## Reasoning

SoleLoop's data is inherently event-based, an append-only sequence of
facts per request, plus a derived summary. This fits a key-value
structure directly, without needing joins across related tables.

DynamoDB's on-demand pricing means paying only for actual requests,
with no baseline cost at zero usage. RDS and Aurora typically require
VPC placement and incur cost even when idle. They also introduce a
real operational risk under Lambda: relational databases have finite
connection limits, and concurrent Lambda invocations can exhaust
them without additional infrastructure (such as RDS Proxy) to manage
pooling.

DynamoDB is not chosen because it is "better than Postgres." It is
chosen because it fits SoleLoop's actual constraints: event-based
data, low traffic, serverless deployment, no VPC, and minimal
operations.

## Alternatives Considered

- **RDS (PostgreSQL/MySQL).** Rejected. Requires VPC placement,
  ongoing instance cost regardless of usage, and connection pooling
  considerations under concurrent Lambda invocations.
- **Aurora Serverless.** Rejected. Narrows the cost gap versus
  standard RDS but still carries more baseline cost and operational
  complexity than DynamoDB at this project's scale.

## Revisit If

- SoleLoop's data model grows to require genuine relational
  querying, complex joins across many related entity types, that a
  key-value structure can no longer serve cleanly.
