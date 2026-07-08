# ADR 020: GitHub Actions Authenticates via OIDC, Not a Long-Lived IAM User

## Context
CI/CD needs a way to let GitHub Actions run Terraform against AWS. The
obvious approach is a dedicated IAM user with an access key and secret
key, stored as GitHub Secrets. That's a real improvement over reusing
personal admin credentials, but it's still a long-lived credential that
sits there indefinitely until someone manually rotates or revokes it.

## Decision
GitHub Actions authenticates using OIDC (OpenID Connect) federation
instead. GitHub issues a short-lived, signed token at the moment a
workflow runs. AWS checks that token against a trust policy scoped to
this specific repo, and if it matches, hands back temporary credentials
that expire quickly. No access key or secret key is ever stored
anywhere.

## Reasoning
A leaked access key is usable by anyone who has it, from anywhere,
until someone notices and rotates it. OIDC removes that risk entirely,
since there's no standing secret to leak in the first place. This is
also AWS's own current recommended practice for GitHub Actions
specifically, not just a defensive choice.

## Alternatives Considered
- Dedicated IAM user with access keys in GitHub Secrets. Better than
  reusing personal credentials, but still a long-lived secret that
  needs rotation and could leak.

## Revisit If
GitHub or AWS meaningfully changes how OIDC federation works, or if
this project ever needs to run Terraform from somewhere that can't use
OIDC (a different CI system without OIDC support, for example).