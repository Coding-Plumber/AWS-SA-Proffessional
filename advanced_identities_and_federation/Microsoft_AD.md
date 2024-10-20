# AWS Directory Service (Microsoft AD)

## Core Features
- Built using Microsoft Active Directory 2012 R2
- Managed using standard Active Directory tools
- Supports Group Policy and Single Sign-On (SSO)
- Supports schema extension - MS AD-aware apps
- Compatible with SharePoint, SQL, Distributed File System (DFS)
- Two sizes:
  - Standard (up to 30,000 objects)
  - Enterprise (up to 500,000 objects)

## Usage and Management
- Used for AD authentication/authorization of products and services within AWS
- Highly available by default (2 AZs)
- Includes monitoring, recovery, replication, snapshots, and maintenance
- Configurable; managed by AWS
- Supports one-way AND two-way external and forest trusts with on-premises Active Directory
- Can operate through network-like failure to any connected on-premises systems
- Supports RADIUS-based MFA

## Infrastructure
- Run from AWS-managed VPC
- Injected into a customer-managed VPC
- Specifically uses 2 subnets within a VPC
- In each subnet, an ENI is added which belongs to one of the domain controllers
- Domain controllers replicate data between each other for high availability
- Automatic patching and maintenance

## AWS Managed AD Mode
- Full, native Microsoft AD within AWS
- Has its own users and objects
- Supports AWS services requiring directory service:
  - Console, WorkSpaces, WorkMail, Connect, RDS, Chime, WorkDocs, QuickSight

## Unique Features
- Only type supporting one-way and two-way trusts with existing on-premises Active Directory
- Supports schema extensions
- Supports apps requiring native Microsoft AD (Remote Desktop, SharePoint, SQL Server, DFS)
- MFA via integration with existing RADIUS server or infrastructure
- Identity Federation with cloud platforms like Azure AD or Microsoft 365

## Exam Tips
- For trusts: must be AD mode
- MFA RADIUS: could be AD mode or AD Connector
- Schema extensions: must be AD mode
- Large numbers or native Microsoft AD needs: AD mode
- RADIUS-based MFA: AD mode or Connector
- Choose between AD mode and AD Connector based on:
  - If you don't want directory data in AWS: use AD Connector
  - If you need to continue running if IP network link fails between AWS and on-premises: use Microsoft AD mode

## Best Choice
- For more than 5,000 users
- If you need trust relationships between AWS and on-premises directories

# AD Connector

## Core Features
- Pair of directory endpoints running in AWS (ENIs in a VPC)
- Supports directory-aware AWS products
- Redirects requests to existing on-premises directory services
- No directory data stored in AWS; all requests redirected
- Enables use of existing on-premises AD with directory-compatible AWS services without storing identity data in AWS
- Suitable for proof of concepts or when existing identities must be used

## Usage and Management
- Allows linking AWS services (e.g., WorkSpaces) with on-premises AD
- Acts as a relay for authentication requests to on-premises directory
- Does not perform authentication or other directory workloads itself
- Available in two sizes:
  - Small
  - Large
- Size controls allocated compute resources, not specific user limits
- Multiple AD Connectors can be used to distribute load
sudo apt install chromium-browser
## Infrastructure
- Requires 2 subnets within a VPC, in different AZs
- Needs one or more on-premises directory servers to be configured
- Requires working network connection between AWS VPC and configured directory servers
- Network connectivity typically via Direct Connect or VPN

## Unique Features
- Allows quick concept connections to on-premises AD
- Ideal when directory data cannot be stored in AWS
- Supports RADIUS-based MFA (similar to AWS Managed Microsoft AD)

## Limitations
- Requires stable network connectivity to function
- Cannot operate independently if connection to on-premises AD is lost

## Exam Tips
- Choose AD Connector when:
  - A quick proof-of-concept connection is needed
  - Directory data absolutely cannot be stored in AWS
  - There's a compelling reason not to use AWS Managed Microsoft AD
- Remember that network connectivity is crucial for AD Connector to function
- For MFA via RADIUS, both AD Connector and AWS Managed Microsoft AD are options

## Best Choice
- For scenarios where keeping all directory data on-premises is a strict requirement
- When AWS services need to use existing on-premises AD identities without replication to the cloud
- For organizations with less than 5,000 users (for larger user bases, AWS Managed Microsoft AD might be more suitable)

# Comparison Notes
- AWS Managed Microsoft AD stores a full copy of the directory in AWS, while AD Connector does not store any directory data
- AWS Managed Microsoft AD can operate independently of on-premises infrastructure, while AD Connector always requires a connection to on-premises AD
- Both support RADIUS-based MFA
- AWS Managed Microsoft AD supports trust relationships and schema extensions, while AD Connector does not
- Choose based on specific requirements for data storage location, scale, and independence from on-premises infrastructure
