# ADR 021: Scoped CI IAM Policy for GitHub Actions Terraform Role

## Context
The GitHub Actions OIDC role (GitHubActions-SoleLoopTerraform) started with wildcard permissions across DynamoDB, Cognito, IAM, Lambda, and API Gateway. That was always meant to be temporary, a way to prove CI/CD worked end to end before locking it down. Once the pipeline was confirmed working, it was time to scope it.

## Decision
Replaced the wildcard policy with a hand-scoped one, built by going through each Terraform resource and granting only what it actually needs, tied to specific ARNs wherever AWS allows it. Three exceptions stayed unscoped by resource, and I've called those out explicitly below. I also left the CI role's own management (the OIDC provider, the role, this policy) out of what CI can write to, so a compromised workflow can't rewrite its own permissions. Two narrow read-only additions let CI still refresh those two resources during a plan.

## Reasoning
I wanted to use IAM Access Analyzer to generate this from CloudTrail history, but this account has no Trail set up, so that wasn't available. Setting one up just for this felt like overkill for now. So I built the policy by hand instead, action by action, then tightened it through trial and error: apply, push, watch terraform plan fail in CI with a specific AccessDenied error, add exactly that action, repeat. Took six rounds.

What I learned along the way: referencing an existing IAM role, even just to read it, needs four separate actions (GetRole, ListRolePolicies, GetRolePolicy, ListAttachedRolePolicies). Lambda and Cognito each have a few extra read calls too (ListVersionsByFunction, GetFunctionCodeSigningConfig, GetUserPoolMfaConfig) that only show up once you clear the earlier blockers. None of that is obvious from the docs, you find it by running it.

## Alternatives Considered
- Set up a CloudTrail Trail so Access Analyzer could generate this: skipped for now, that's its own S3 bucket and ongoing cost for a one-time task.
- Keep the wildcard policy: no, that's the whole thing I was trying to fix.
- Scope every single action to a specific resource with zero exceptions: not possible. API Gateway actions and cognito-idp:CreateUserPool can't be resource-scoped, the API or pool doesn't exist yet at creation time. Those two gaps are accepted and documented, not missed.

## Revisit If
- A CloudTrail Trail gets set up for other reasons later, worth running Access Analyzer against this policy then.
- The CI role's own setup (github-oidc.tf) needs to change, that stays a manual apply by design, not something CI runs.
- AWS adds resource-level scoping for API Gateway or Cognito pool creation, tighten those two statements then.