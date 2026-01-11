# ADR-001: Transition from Manual Provisioning to Infrastructure-as-Code (Terraform)

## Status

*Accepted*

## Context

As part of the 2026 Cloud Architecture pivot, the initial January baseline required standing up a VPC and EC2 instance. The goal was to establish a secure, repeatable environment for future AI (RAG) deployments.

In Week 1, a manual build was performed via the AWS Management Console to understand service dependencies (VPC -> Subnets -> IGW -> Route Tables).

## Decision

We have decided to move all foundational infrastructure management to HashiCorp Terraform. This transition ensures that the "Manual-to-IaC" roadmap is met and provides a professional baseline for the rest of the 2026 training plan.

## Technical Requirements

- Tooling: Terraform CLI + AWS CLI.
- Provider: AWS (HashiCorp verified).
- Architecture: VPC with Public Subnet, IGW, and EC2 (t3.micro for cost-efficiency).

## Rationale

Repeatability: Manual configuration is prone to human error and "configuration drift."

Version Control: Storing infrastructure in GitHub allows for peer review and historical tracking.

Security (CISO Alignment): Defining Security Groups and IAM Roles in code ensures that the "Principle of Least Privilege" is consistently applied.

Efficiency: "Terraform Destroy" ensures no orphan resources are left running, directly supporting our FinOps goals.

## Consequences

Positive: Infrastructure is now documented as code; environment can be destroyed and recreated in minutes.

Negative: Requires state file management (handled via .gitignore for this phase).

Next Steps: February RAG infrastructure will build upon these Terraform modules.