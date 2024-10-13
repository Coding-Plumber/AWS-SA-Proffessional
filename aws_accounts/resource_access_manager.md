# AWS Resource Access Manager (RAM)

## Overview
- Allows sharing of AWS resources between accounts
- Only supported by specific AWS services
- Resources can be shared with:
  - Individual AWS accounts
  - Organizational Units (OUs)
  - Entire AWS Organizations
- Shared resources are accessible natively (via AWS CLI or Console)
- No additional charge for using RAM (standard service costs apply)
- Enables significant changes to traditional AWS architecture

## Availability Zone (AZ) Considerations
- AWS rotates which physical facilities are used for AZs
- AZ designations (e.g., eu-west-2a) may not be consistent across accounts
- Coordinating resources between accounts for performance or high availability can be challenging
- Solution: Use AZ IDs for consistency
  - Example: `use1-az1` is always the same physical AZ across accounts
  - Use AZ IDs when deploying shared infrastructure across different accounts
- This system helps distribute load across regions and enhances security

## RAM Sharing Process
1. Resource owner creates a share and provides a name
2. Owner retains full ownership of shared resources
3. Owner defines the principal (account, OU, or organization) to share with
4. If the participant is within an AWS Organization with sharing enabled, acceptance is automatic
5. For accounts outside the organization or when organization sharing is disabled, an invite must be accepted

## Shared VPC Infrastructure
- VPCs can be created to provide shared infrastructure services to other AWS accounts
- Participants can:
  - Provision services into shared subnets
  - Read and reference network objects
  - Cannot modify or delete shared network resources
- Resource ownership:
  - The account provisioning a resource in a shared subnet owns that resource
  - Example: If Account 3 provisions a resource in Account 1's shared subnet, Account 3 owns the resource
- Resource visibility:
  - Accounts can only see and manage their own resources in shared subnets
  - Example: Account 2 cannot see EC2 instances provisioned by Account 3 in the same shared subnet

## Owner and Participant Perspectives
- Owner account:
  - Owns the VPC and subnets
  - Can view and modify VPC and subnet configurations
- Participant accounts:
  - Can view and reference shared subnets
  - Can use shared subnets in security groups
  - Can provision resources into shared subnets
  - Own the resources they provision
  - Cannot see or modify resources created by other accounts

## Resource Sharing Limitations
- Some resources can be shared with any AWS account
- Other resources can only be shared within an AWS Organization

## Key Benefits
- Enables network connectivity while maintaining separation in management consoles and CLI
- Facilitates centralized network management and distributed resource deployment
