# 013. VS Admin Override on Approval Decisions

## Context
The Mayor is the sole City Approver by policy decision. In practice, the Mayor's availability and response time by email are limited, which can stall a request that is otherwise ready to move forward. Waiting solely on the Mayor for every decision risks delaying the platform's own milestone-tied delivery schedule.

## Decision
Allow VS Admin to approve or reject a request in addition to the City Approver, rather than making approval authority exclusive to the Mayor.

## Reasoning
VS Admin already owns scope classification, assignment, completion, and archiving, and is accountable for keeping milestones on schedule. Giving VS Admin override power on approval decisions prevents a single person's availability from becoming a bottleneck on a process VS Admin is already responsible for keeping on track. This is a trust and business decision, not just a technical one, so it is documented separately from ADR 012, which only covers where authorization is enforced.

## Alternatives Considered
Requiring every approval to go through the Mayor with no override. Rejected because it introduces a single point of failure tied to one person's calendar and availability, with no fallback if the Mayor is slow or unreachable.

## Revisit If
The Mayor's office designates a formal delegate or backup approver, or a client relationship requires strict single-approver enforcement with no VS Admin override. Either case would change this decision, not just extend it.
