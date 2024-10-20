# AWS Control Tower

## Overview
- Quick and easy setup of multi-account environment
- Orchestrates other AWS services to provide functionality
- Uses AWS Organizations, IAM Identity Center, CloudFormation, Config, and more
- Evolution of AWS Organizations with additional features, intelligence, and automation

## Landing Zone
- Multi-account environment (like AWS Organizations with enhanced capabilities)
- Features:
  - SSO/ID Federation
  - Centralized logging and auditing
- When creating a Control Tower, it becomes the management account of the landing zone

## Components
1. **Guardrails**: Detect/Mandate rules and standards across all accounts
2. **Account Factory**: Automates and standardizes new account creation
3. **Dashboard**: Single-page oversight of the entire environment

## Structure
- AWS Organization with default Foundational OU + Custom OU
- Foundational OU contains two accounts:
  1. Audit Account: For users needing audit info (e.g., SNS + CloudWatch for monitoring Landing Zone)
  2. Log Archive Account: For access to all logging info (e.g., Config + CloudTrail)

## Account Factory
- Creates provisioned setup accounts
- Configures account + network baseline templates ("Cookie Cutter")
- Uses CloudFormation stacks under the hood
- Employs both Config and SCPs for account guardrails

## Landing Zone Details
- Well-architected multi-account environment in the home region
- Built with AWS Organizations, AWS Config, CloudFormation
- Security OUs: Log Archive & Audit accounts (CloudTrail + Config)
- Sandbox OU: For testing with less rigid security
- Customizable: Can create other OUs and Accounts
- IAM Identity Center (AWS SSO): SSO, multiple accounts, ID Federation
- Monitoring and notifications: CloudWatch + SNS
- End-user account provisioning via Service Catalog

## Guard Rails
- Rules for multi-account governance
- Categories: mandatory, strongly recommended, elective
- Types:
  1. Preventive: Stop you from doing things
     - Status: Enforced or not enabled
     - Examples: Allow/deny regions, disallow bucket policy changes
  2. Detective: Compliance checks (AWS Config rules)
     - Status: Clear, in violation, or not enabled
     - Examples: Detect CloudTrail enabled, EC2 public IPv4

## Account Factory Details
- Automated account provisioning
  - For cloud admins or end users (with permissions)
- Guardrails automatically added
- Account admin assigned to a named user (IAM Identity Center)
- Standard configurations for account and network
- Accounts can be closed or repurposed
- Can be fully integrated with a business SDLC (Software Development Lifecycle)
