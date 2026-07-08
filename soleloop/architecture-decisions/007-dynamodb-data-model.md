# 007: DynamoDB Data Model (CQRS Split)

## Context

Now that we've picked DynamoDB, we need a way to keep a full history
of every change request while still being able to check a request's
current status quickly. Trying to do both in one table would force
tradeoffs we don't want.

## Decision

Two tables:

- `SoleLoopEventLog` — the full history. Every event ever recorded,
  never edited or deleted.
- `SoleLoopProjection` — just the current status of each request,
  kept up to date as new events come in.

## Reasoning

Every time something happens to a request (submitted, approved,
started, completed), it gets written to the event log and stays
there permanently. That log is our real record, if there's ever a
question about what was asked for and when, this is the proof.

The projection table exists so we're not re-reading the entire
history every time someone just wants to know where a request
currently stands. It's a quick-reference view, one row per request.

Each event gets a sort key combining the timestamp, the event type,
and a unique ID, so events always come back in the right order, and
two events happening in the same second never overwrite each other.

## Alternatives Considered

- **One table for everything.** Rejected. Would mean replaying the
  full history every time we just want current status, or writing
  more complicated logic to update one row while still keeping
  history intact.
- **Simple numbered IDs (1, 2, 3...).** Rejected. Would need extra
  logic to make sure two requests never accidentally get the same
  number at the same time. A generated unique ID avoids that problem
  entirely.

## Revisit If

- Our actual usage patterns change enough that the current-status
  table stops matching how we're really querying it.
