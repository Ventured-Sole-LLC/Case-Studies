# ADR 019: HTTP API Needs an Explicit Stage in Terraform

## Context
Every piece of API Gateway looked right in the console, the routes, the
integrations, the authorizer, but the first test came back "Not Found."
Turns out nothing was actually reachable.

## Decision
Added an aws_apigatewayv2_stage resource named $default, with
auto_deploy set to true.

## Reasoning
Building an HTTP API in the console quietly creates and deploys a
$default stage for you, so it's easy to forget that's a separate thing
that has to exist. Terraform doesn't do that automatically. Routes and
integrations can be fully built and still go nowhere without a stage.
auto_deploy true means future changes republish automatically, so this
isn't a step I have to remember to repeat.

## Alternatives Considered
- Deploy manually after every apply. No, reintroduces the exact manual
  step Terraform is supposed to remove.
- A named stage instead of $default. Considered, but $default matches
  the real API and keeps the invoke URL simple for now.

## Revisit If
This API ever needs multiple live stages at once, like a real
staging/prod split.
