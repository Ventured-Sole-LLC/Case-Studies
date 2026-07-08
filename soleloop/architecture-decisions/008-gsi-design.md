# 008: Access Patterns and GSI Design

## Context

The current-status table (SoleLoopProjection) needs to answer two
questions that don't start from a known request: "show me one
requester's own requests" and "show me everything waiting on
approval" (the mayor's queue). The event log only ever gets asked one
question: "show me everything for this specific request."

## Decision

Two GSIs (Global Secondary Indexes) added to SoleLoopProjection only:

- requester-index, looks things up by requester.
- status-index, looks things up by status (pending, approved, and
  so on).

No indexes were added to the event log.

## Reasoning

A GSI is basically a second, automatically kept-up-to-date copy of a
table, organized around a different question. Without one, DynamoDB
has to check every single row to answer a query. That's slow, and it
only gets slower as the table grows.

The event log only ever gets asked one question, and its own key
already answers it directly, so no index is needed there.

The current-status table genuinely gets asked two other real
questions, so it gets one index per question. Both indexes also sort
by time, so results come back already in order, oldest first, without
extra work later.

## Alternatives Considered

- Adding indexes to the event log too, just in case. Rejected. No
  real question needs one right now, and an index we don't use is
  just added cost with nothing to show for it.

## Revisit If

- A new real question comes up that isn't already answered by an
  existing key or index, for example, filtering requests by category
  or urgency.
