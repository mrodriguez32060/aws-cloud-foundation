# ADR-001: Transition from Manual Provisioning to Infrastructure-as-Code (Terraform)

## Status
**Accepted**

## Context
As part of the 2026 Cloud Architecture pivot, the initial January baseline required standing up a VPC and EC2 instance. The goal was to establish a secure, repeatable environment for future AI (RAG) deployments. 

In Week 1, a manual build was performed via the AWS Management Console to understand service dependencies (VPC -> Subnets -> IGW -> Route Tables). In Week 2, this was transitioned to an automated deployment.

## Decision
We have decided to move all foundational infrastructure management to **HashiCorp Terraform**. This transition ensures that the "Manual-to-IaC" roadmap is met and provides a professional baseline for the rest of the 2026 training plan.

## Technical Requirements
- **Tooling:** Terraform CLI + AWS CLI v2 (Installed on Pop!_OS).
- **Provider:** AWS (HashiCorp verified).
- **Architecture:** VPC with Public Subnet, Internet Gateway (IGW), and EC2 Instance.

## Rationale
1. **Repeatability:** Manual configuration is prone to human error and "configuration drift." 
2. **Version Control:** Storing infrastructure in GitHub allows for peer review and historical tracking.
3. **Security (CISO Alignment):** Defining Security Groups and IAM Roles in code ensures that the "Principle of Least Privilege" is consistently applied from the start.
4. **Efficiency:** Using `terraform destroy` ensures no "orphan" resources are left running, directly supporting our FinOps goals and account cleanliness.

## Consequences
- **Positive:** Infrastructure is now fully documented as code; the environment can be destroyed and recreated in minutes.
- **Negative:** Requires strict state file and binary management to avoid repository bloat. 
- **Operational Note (Git):** Encountered and resolved Git push failures caused by indexed `.terraform` binaries. Resolved by re-initializing the repository with a robust `.gitignore` and force-pushing a clean history—a key lesson in maintaining lean, professional repositories.
- **Cost Management (FinOps):** Refactored the workshop's default `m5.large` instance to a `t3.micro`. This reduced the hourly burn rate by approximately 90% while maintaining sufficient performance for baseline testing.

## Next Steps
- February RAG infrastructure will build upon these foundational Terraform modules.
- Week 3 will focus on hardening these resources via IAM policy refinements and security group restrictions.