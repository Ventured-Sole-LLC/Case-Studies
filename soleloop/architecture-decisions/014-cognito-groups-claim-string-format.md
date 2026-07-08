# 014. Cognito Groups Claim Is a String, Not a List

## Context
ApproveRequestLambda needs to read the caller's Cognito groups from the JWT claims that API Gateway attaches to the event. Before writing that logic, the actual event was inspected in CloudWatch using a temporary print statement, since assuming the shape without checking had already caused a bug once before in ADR 010.

## Decision
Parse cognito:groups as a plain string, stripping the brackets and splitting on commas, rather than assuming it arrives as a real JSON array.

## Reasoning
The CloudWatch log confirmed cognito:groups comes through looking like a list but is actually just text, for example "[vs-admin]". Treating it as a real list and checking membership with something like if 'vs-admin' in caller_groups would often seem to work, since Python's in operator also matches a smaller string inside a bigger one. That coincidence hides a real risk. If a future group name were ever a substring of another, or the bracket text were checked carelessly, the membership check could pass or fail incorrectly without any obvious error. Stripping the brackets and splitting into an actual list removes that risk instead of relying on a lucky match.

## Alternatives Considered
Trusting the claim's shape without checking it first, based on how JWT claims are usually described. Rejected, since this is the second time in this build that an assumed event shape turned out to be wrong once actually inspected, and the fix here is small and permanent.

## Revisit If
API Gateway or Cognito ever changes how group claims are serialized, or a future Lambda pulls this same claim and finds a different format. At that point the parsing logic here should be checked again rather than copied forward blindly.
