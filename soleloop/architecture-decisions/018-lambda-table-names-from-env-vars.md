# ADR 018: Lambda Table Names Read from Environment Variables

## Context
Both Lambdas had the DynamoDB table names hardcoded in the Python code.
This bit us while testing the dev- Terraform environment: the dev
Lambdas had IAM permissions scoped to the dev- tables, but the code was
still trying to write to the plain production names. IAM correctly
blocked it. That's when I noticed the code had no way to point at a
different environment without editing it directly.

## Decision
Both functions now read table names from environment variables,
falling back to the original production names if nothing's set.

## Reasoning
Production doesn't change at all unless I set those variables there
too. The dev environment can now point at its own tables just by
setting two values in lambda.tf, no code edits needed. Also just fixes
a bad habit. Hardcoding resource names in application code is fragile
regardless of whether a second environment exists.

## Alternatives Considered
- Grant dev Lambdas access to the real production tables instead. No.
  Defeats the whole point of having an isolated dev environment.
- Only ever test against production directly. Also no, same reason.

## Revisit If
A staging environment gets added later. Worth checking then whether two
env vars is still enough or if a small config module makes more sense.
