# 004: No Client Secret on the Cognito App Client

## Context

Cognito offers the option to generate a client secret when creating
an app client.

## Decision

The app client was created without a client secret.

## Reasoning

SoleLoop's frontend is a browser-based single-page app. Anything
shipped to a browser can be read by the user, so a secret embedded
in frontend code isn't actually secret. Enabling one here would
either leak into a public JS bundle or break login entirely, since
the frontend has no safe way to supply it.

The correct pattern for a browser app is Authorization Code Grant
with PKCE, which proves the request came from the same client
without requiring a stored secret.

## Alternatives Considered

- **Client secret enabled.** Rejected. No real security benefit for
  a public client, and a common source of leaked secrets or broken
  logins if left on by default.

## Revisit If

- SoleLoop adds a server-side service that calls Cognito directly
  (not through a user's browser). That component would get its own
  app client with a secret, kept server-side only.
