# ADR 017: dev- Prefix for Everything Terraform Creates

## Context
I'm building the Terraform version of SoleLoop in the same AWS account
and same region (us-east-2) as the real, live the live municipal client resources.
That's on purpose, I want this to be a faithful rebuild, not a different
setup. But it means if I'm not careful, Terraform is going to try to
create something with the exact same name as something that's already
running in production. DynamoDB actually won't let that happen (table
names are unique per account/region), but I'd rather not find that out
by accident mid-apply, and some other services won't be as forgiving.

## Decision
Every single resource Terraform creates gets a dev- prefix. The
DynamoDB table is dev-SoleLoopEventLog, not SoleLoopEventLog. Same
pattern going forward for Cognito, Lambda, API Gateway, all of it.

## Reasoning
This is the simplest fix that actually works. I don't need a second AWS
account, I don't need new credentials to manage, I don't need to think
about cross-account IAM while I'm still learning the basics of IAM
itself. A prefix costs nothing and it makes it obvious at a glance, in
the console or in the CLI, which resources are the real thing and which
ones are me practicing.

## Alternatives Considered
- A whole separate AWS account: real isolation, but it's a chunk of
  extra setup that has nothing to do with what I'm actually trying to
  learn right now.
- Same account, different region: avoids the name collision, but it
  quietly undoes the region decision I already made and wrote down in
  ADR 003, for no real reason. And it wouldn't fully solve this anyway,
  since some resource names (like S3 buckets) have to be unique
  globally, not just per region.

## Revisit If
This ever stops being a practice environment and turns into something
real, like a second environment for an actual second client. At that
point dev- should get swapped for something meaningful (staging-, or
the client's name) and that swap gets its own ADR.
