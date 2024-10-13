## AWS Organisations 

## Overview
AWS Organizations is a service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage.

## Key Concepts

### Management Account
- Previously called "master account"
- Used to create the organization
- Cannot be changed after organization creation
- Responsible for paying all charges accrued by the member accounts

### Member Accounts
- Standard AWS accounts that are part of an organization
- Can be existing accounts invited to the organization or new accounts created within the organization

## Organizational Structure

### Organization Root
- Top-level container for all accounts in an organization

### Organizational Units (OUs)
- Containers for accounts within the organization
- Can contain other OUs, creating a hierarchical structures


Organization Root
├── Management Account
├── OU 1
│   ├── Member Account A
│   └── Member Account B
├── OU 2
│   ├── Sub-OU 2.1
│   │   └── Member Account C
│   └── Member Account D
└── Member Account E (directly under root)

# Features

### Consolidated Billing
- All member accounts' charges are paid by the management account
- Often referred to as the "payer account"
- Enables volume discounts across all accounts in the organization

### Account Creation
- New accounts can be created directly within the organization
- No invitation process needed for accounts created this way

## Best Practices

### Central Identity Account
- Create a dedicated account within the organization for identity management
- This account hosts IAM users and roles for the entire organization
- Users log into this central account and assume roles to access other accounts
- Enhances security and simplifies access management

### Identity Federation
- Can be used with the central identity account
- Allows for integration with external identity providers

### Role Switching
- Users can switch roles to access different accounts within the organization
- Implements the principle of least privilege

## Benefits
- Centralized management of multiple AWS accounts
- Consolidated billing and potential cost savings
- Hierarchical grouping of accounts for easier organization
- Simplified security management through centralized identity control


