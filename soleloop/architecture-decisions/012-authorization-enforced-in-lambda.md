# 012. Where Group Authorization Is Enforced

## Context
API Gateway's JWT authorizer confirms a request carries a valid, unexpired Cognito token. It does not check which group the caller belongs to. A separate decision was needed on where to enforce the actual rule: only city-approver or vs-admin can approve or reject a request.

## Decision
Enforce the group check inside the Lambda function itself, by reading cognito:groups off the ID token claims, rather than relying on API Gateway to filter by group.

## Reasoning
API Gateway's authorizer answers one question: is this a real, valid login. It does not answer whether this specific person is allowed to do this specific action. Keeping the group check inside the Lambda puts the business rule in one place, next to the rest of the decision logic like status validation and state checks. It is easier to debug and easier to explain, since the rule lives in plain Python instead of being split across console configuration and code.

## Alternatives Considered
Configuring API Gateway to enforce authorization scopes at the route level, rejecting non-approver calls before they reach the Lambda. Rejected for now since it moves part of the business rule out of the code and into configuration, which is harder to see and document. Worth revisiting if performance or defense in depth calls for rejecting bad requests earlier.

## Revisit If
The number of roles or approval rules grows complex enough that repeating this check across many Lambda functions becomes error prone. At that point a shared authorization layer, like a Lambda authorizer or a shared utility function, may be worth building instead of repeating the check inline.
