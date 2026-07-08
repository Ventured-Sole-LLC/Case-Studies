# 002: Cloud Provider Selection (AWS)

## Context

Government and municipal environments commonly run on Microsoft Azure,
often tied to an existing Microsoft 365 or Azure AD tenant used for
staff email and internal systems. That made Azure the real alternative
to weigh against AWS here, not a generic "which cloud" choice.

The client has no dedicated tech team, and the point of contact has no
working knowledge of what infrastructure, if any, the city currently
runs internally. No municipal contract or procurement document
specifies a required platform. Given the norm in gov-sector IT, it's
reasonably likely the city's internal environment, if any exists, is
Microsoft-based.

## Decision

SoleLoop runs entirely on AWS.

## Reasoning

AWS was chosen primarily because it matches the certification and
skill-building path already in progress. Building a real, live client
project on the platform being certified in produces a stronger outcome
than building on a platform chosen for pure technical reasons, the
architecture gets tested against a real client, not a lab exercise.
This is a legitimate basis for the decision and is stated plainly
rather than dressed up as a purely technical conclusion.

This decision does carry a real tradeoff. If the city's internal
environment turns out to be Azure AD-based, native integration would
have been simpler on Azure. AWS is not incapable here, Cognito can
federate with Azure AD through SAML or OIDC, allowing city staff to
log in with existing Microsoft credentials, but this requires
deliberate setup and ongoing maintenance rather than being automatic.
Until or unless that federation is built, city staff would hold a
separate SoleLoop-specific login, one more credential for a client
with no dedicated tech team to manage on their end.

No confirmed constraint on the client side currently forces either
provider, so this tradeoff was accepted knowingly rather than
overlooked.

## Alternatives Considered

- **Azure.** The likely native fit given gov-sector norms and possible
  existing Microsoft 365/Azure AD use. Not chosen due to the
  certification-path reasoning above, with the federation tradeoff
  above accepted as a known cost.
- **Multi-cloud.** Never seriously considered. Solves problems (vendor
  lock-in at scale, contractual multi-provider requirements) this
  project doesn't have.

## Revisit If

- The client confirms an existing Azure AD or Microsoft 365 tenant,
  making Cognito-to-Azure AD federation worth building.
- A future municipal contract explicitly specifies a required cloud
  platform.
- A second build of this same architecture on Azure (planned as a
  separate portfolio project) reveals that Azure-native gov integration
  is meaningfully simpler in practice, informing which platform to
  default to for future municipal clients.
