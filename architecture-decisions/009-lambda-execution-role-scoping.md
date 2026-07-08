# 009: Lambda Execution Role Scoping and Validation Design

## Context

SubmitRequestLambda needed an execution role and a way to handle
missing input before writing to DynamoDB.

## Decision

We built a custom, scoped IAM policy instead of using
AmazonDynamoDBFullAccess. The role can only PutItem, GetItem, and
UpdateItem on the two SoleLoop tables. The function also checks that
requester_id and description are present before writing anything.

## Reasoning

AmazonDynamoDBFullAccess would let this function touch every table
in the account. A scoped policy limits the damage of a bug or leaked
credentials to just these two tables and three actions.

Checking input first avoids half-written data. If something's
missing, the function stops and returns a clear error instead of
writing an incomplete record.

## Alternatives Considered

- AmazonDynamoDBFullAccess. Rejected, far more access than needed.
- No input validation. Rejected, risks partial writes and unclear
  errors.

## Revisit If

- More Lambda functions need different or broader DynamoDB access.
  Each should get its own scoped role.
