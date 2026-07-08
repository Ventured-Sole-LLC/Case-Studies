# 016. Terraform as a Parallel Learning Environment

## Context
SoleLoop's AWS resources were built manually through the console first, then documented, following the established pattern of learning a service before automating it. The next step is recreating this system in Terraform. The question was whether to import the existing live resources into Terraform state right away, or build a separate parallel environment first.

## Decision
Build Terraform as a completely separate learning environment, in its own local folder outside the SoleLoop repo, without importing or touching the live the live municipal client resources. Also reuse the existing default AWS CLI profile for now instead of creating a dedicated Terraform IAM identity, with the explicit intent to harden this once the Terraform structure works.

## Reasoning
Importing existing resources into Terraform state is a real skill, but it adds risk and complexity on top of learning Terraform's basic mechanics for the first time. Keeping this environment separate means terraform apply and terraform destroy can be run freely while learning, with zero risk to the live, working the live municipal client system. Reusing the default profile for now follows the same sequencing already used throughout this project: get something working first, then document and apply a tighter security decision afterward, rather than adding IAM complexity before there is anything running to protect.

## Alternatives Considered
Importing the live resources immediately and writing Terraform against the real environment from the start. Rejected for this phase, since a mistake in unfamiliar Terraform syntax could affect resources the live municipal client is actively depending on.
Creating a dedicated Terraform IAM user before writing any configuration. Deferred, not rejected. This is still the right long-term move, just not the right first move.

## Revisit If
The Terraform configuration is working and understood well enough to consider importing the real SoleLoop resources, or before any Terraform command is ever run against the live AWS account instead of this separate learning environment.
