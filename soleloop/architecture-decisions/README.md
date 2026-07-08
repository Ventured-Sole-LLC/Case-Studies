# Architecture Decision Records: Reading Guide

ADRs are numbered in the order they were written and committed, not in
the logical order a new team member would want to read them. Numbers
are permanent identifiers and are never reused or reordered, even if a
decision made later would have made more sense earlier in the story.

For onboarding or portfolio review, please read them in this order instead:

1. [002 - Cloud Provider Selection](002-single-cloud-aws.md)
2. [003 - Region Selection](003-region-selection.md)
3. [001 - Single Environment](001-single-environment-no-staging.md)
4. [004 - Cognito No Client Secret](004-cognito-no-client-secret.md)
5. [011 - Using the ID Token for Authorization](011-id-token-for-authorization.md)
6. [012 - Where Group Authorization Is Enforced](012-authorization-enforced-in-lambda.md)
7. [013 - VS Admin Approval Override](013-vs-admin-approval-override.md)
8. [014 - Cognito Groups Claim Is a String, Not a List](014-cognito-groups-claim-string-format.md)
9. [015 - One Endpoint for Approve and Reject](015-single-decision-endpoint.md)
10. [005 - Multi-Requester Design](005-multi-requester-design.md)
11. [006 - Database Selection](006-database-selection.md)
12. [007 - DynamoDB Data Model](007-dynamodb-data-model.md)
13. [008 - Access Patterns and GSI Design](008-gsi-design.md)
14. [009 - Lambda Execution Role Scoping](009-lambda-execution-role-scoping.md)
15. [010 - API Gateway HTTP API Event Body Parsing](010-api-gateway-http-api-event-body-parsing.md)
16. [016 - Terraform as a Parallel Learning Environment](016-terraform-parallel-learning-environment.md)
17. [017 - Dev Prefix for Terraform Resources](017-dev-prefix-for-terraform-resources.md)
18. [018 - Lambda Table Names From Env Vars](018-lambda-table-names-from-env-vars.md)
19. [019 - HTTP API Needs an Explicit Stage](019-http-api-needs-explicit-stage.md)
20. [020 - GitHub Actions OIDC Auth](020-github-actions-oidc-auth.md)
21. [021 - Scoped CI IAM Policy](021-scoped-ci-iam-policy.md)
For business context, event model, and authorization design (not
architecture decisions in the ADR sense), see:

- `docs/event-model.md`
- [docs/auth-matrix.md](../auth-matrix.md)
- Repository root `README.md`
