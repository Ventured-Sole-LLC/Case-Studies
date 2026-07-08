# 005: Multiple Requesters, Single Approval Gate

## Context

The client has 5 aldermen who may need to submit change requests. We
had to decide whether to allow one designated requester or multiple
concurrent requesters in the `city-requester` Cognito group.

## Decision

Multiple requesters are allowed. Any designated official can be added
to `city-requester`.

## Reasoning

The initial concern with multiple requesters was uncoordinated,
duplicate, or conflicting requests recreating the same chaos this
project exists to fix. Two things make multiple requesters safe here:

1. Every event carries a `requester_id`, so no request is ever
   ambiguous about who submitted it, regardless of how many people
   can submit.
2. The mayor is the sole approver. Nothing becomes actual work until
   he approves it, which is the real coordination point, not
   something the requester layer needs to enforce on its own.

Restricting to a single requester would have added friction without
solving a problem the approval gate doesn't already solve.

## Alternatives Considered

- **Single designated requester only.** Rejected. Unnecessarily
  restrictive given the approval gate already prevents uncoordinated
  work, and doesn't reflect how the city is actually structured.

## Revisit If

- Near-duplicate requests from different requesters become a
  frequent real problem for the approver to sort through. At that
  point, lightweight duplicate-flagging in the submission flow is a
  cheaper fix than restricting who can submit.
