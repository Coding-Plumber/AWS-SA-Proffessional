## Amazon Workspaces

### Overview
- Desktop-as-a-Service (DaaS) solution for remote work/office scenarios
- Similar to Citrix/Remote Desktop, but hosted by AWS
- Provides consistent desktop experience from anywhere - apps and state persist

### Operating Systems and Sizes
- Windows and Linux
- Various sizes available:
  - Value
  - Standard
  - Performance
  - Power
  - Power Pro
  - Graphics
  - Graphics Pro
- Options with or without AWS-provided apps and custom images

### Pricing and Infrastructure
- Monthly or hourly pricing (plus base infrastructure cost)
- Workspaces use Elastic Network Interfaces (ENIs) in a VPC, leveraging VPC networks
- At-rest encryption using EBS and KMS

### User Management and Authentication
- User Directory Service options:
  - Simple AD
  - AWS Managed Microsoft AD
  - AD Connector
- Used for authentication and user management

### Connectivity and Resources
- Windows Workspaces can access:
  - FSx
  - EC2 Windows resources
  - On-premises resources (via VPN or Direct Connect)

### Key Components and Functionality
- Customers connect via Workspaces Client App using shared gateways
- Client bandwidth included in cost
- Authentication and Streaming Gateways handle management traffic
- Normal traffic flows through to Workspaces instances

### Availability and Options
- Available with Linux or Windows operating systems
- Various performance tiers offered

### Infrastructure Details
- Workspaces and directory services run in AWS-managed VPCs
- ENIs injected into customer-managed VPCs
- Not highly available (HA) like EC2 instances
- Occupy a single subnet/Availability Zone

### Network Architecture
- Includes Directory Services (DS) component
- Uses multiple Availability Zones (e.g., AZA, AZB)
- Incorporates NAT Gateway (NATGW) and Internet Gateway (IGW) for connectivity

### Connectivity
- Supports transit to/from public internet
- Allows connections to on-premises infrastructure
- All external connectivity facilitated via VPC

### Management
- AWS manages underlying infrastructure and gateways
- Customers manage Workspaces instances and directory services

This setup allows organizations to provide secure, managed virtual desktops to their users, with flexibility in operating systems and performance tiers, while leveraging AWS's infrastructure and networking capabilities.
