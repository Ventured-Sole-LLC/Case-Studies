# 011. Using the ID Token for Authorization

## Context
Cognito issues three tokens on login: an ID token, an access token, and a refresh token. ApproveRequestLambda needs to know which Cognito group the caller belongs to, city-approver or vs-admin, before allowing a decision on a request. The two main tokens are not interchangeable for this.

## Decision
Use the ID token, not the access token, as the Bearer token sent to the API and read by the Lambda for authorization checks.

## Reasoning
The ID token carries claims about who the person is, including cognito:groups. The access token is meant for authorizing calls to other services and does not include group membership by default. The core need here is knowing which group the caller is in, and the ID token already carries that without extra setup.

## Alternatives Considered
Using the access token and configuring Cognito to include custom scopes or claims that mirror group membership. Rejected for now since it adds setup for no real benefit when the ID token already has what is needed.

## Revisit If
A future integration needs the Lambda or another service to call additional AWS or third party APIs on the caller's behalf. That is what the access token is actually built for, and both tokens may need to be passed at that point.
