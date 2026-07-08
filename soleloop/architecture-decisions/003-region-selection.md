# 003: Region Selection (us-east-2)

## Context

AWS resources are region-scoped. Before creating any infrastructure,
we needed to settle on a single region and stay in it for the whole
project. The client is based in Texas.

## Decision

All SoleLoop infrastructure runs in us-east-2 (Ohio).

## Reasoning

Region choice for this project came down to four factors: latency to
end users, cost, service availability, and any data residency
requirement.

Cost and service availability are a wash between us-east-1 and
us-east-2 for the services this project uses (Cognito, DynamoDB,
Lambda, API Gateway are all mature and fully available in both). No
compliance or procurement requirement was found specifying a required
region.

That left physical proximity to the end users as the deciding factor.
The client and all its staff are based in Texas. Ohio is geographically
closer to Texas than Virginia, and since this is not a latency-
sensitive real-time application, small physical distance differences
don't meaningfully affect the user experience either way. Given no
other factor differentiated the two options, the closer region was
chosen.

## Alternatives Considered

- **us-east-1 (N. Virginia).** AWS's default and often the first
  region new features launch in. Rejected because nothing in this
  project depends on bleeding-edge features, and it offers no
  advantage over us-east-2 for this client's location.

## Revisit If

- The client base expands to a region where a different AWS region
  would meaningfully reduce latency.
- A future contract specifies a data residency requirement that only
  a specific region satisfies.
