# 015. One Endpoint for Approve and Reject

## Context
A City Approver decision on a request can go one of two ways: approved or rejected. The question was whether to build two separate endpoints, one for approving and one for rejecting, or a single endpoint that takes the decision as part of the request.

## Decision
Use one endpoint, PATCH /requests/{request_id}/decision, with the outcome passed in the request body as decision: APPROVED or decision: REJECTED, instead of two separate endpoints.

## Reasoning
Approval and rejection are the same business operation from the system's point of view: a City Approver making a decision on a pending request. Both follow the exact same rules, the same authorization check, the same state machine protection, and the same event logging pattern, just with a different outcome value. Splitting this into two endpoints would mean duplicating all of that shared logic for no real benefit. Submit creates a request. Decision changes its state. That is the clean line to draw.

## Alternatives Considered
Two separate endpoints, POST /requests/{request_id}/approve and POST /requests/{request_id}/reject. Rejected since it duplicates authorization, validation, and state checking logic across two functions for what is really one action with two possible outcomes.

## Revisit If
Approval and rejection ever need meaningfully different rules, different required fields, or different authorization requirements from each other. At that point splitting them into separate endpoints would make more sense than continuing to force two different operations through one shared shape.
