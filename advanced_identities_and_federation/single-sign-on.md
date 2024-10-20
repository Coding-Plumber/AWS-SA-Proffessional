## AWS Single Sign-On (now IAM Identity) (SSO)

- Manage SSO Access - AWS accounts + External applications 
- Flexible Identity Source 
- AWS SSO - Built-in identity store 
- AWS managed Microsoft Active Directory 
- On-premises MS AD (2-way trust or AD connector) 
- External Identity Provider - SAML 2.0 
- Preferred by AWS vs traditional 'workforce' identity federation

### Exam Tip

If nothing excludes it, SSO is the preferred choice over others such as SAML 2.0.
New deployments should default to utilizing AWS Single Sign-On. This is because SSO, in addition to handling identity federation, also manages permissions across all accounts inside your organization and external applications.


Check wording on Qustion. 
- If work based identity its likely SSO 
- If customer identity ie Google, FB, Twitter, WEB then it wont be AWS SSO. It would most likely be cognito.

### How AWS SSO Works

#### a) Identity Store:
- AWS SSO can use its own built-in identity store or connect to an external source like Active Directory
- It maintains user and group information centrally

#### b) Access Portal:
- Users log in once to a centralized portal
- From this portal, they can access all permitted AWS accounts and applications

#### c) Permission Sets:
- Administrators define permission sets in AWS SSO
- These are collections of policies that determine what users can do in AWS accounts

#### d) Account Assignments:
- Admins assign users or groups to AWS accounts with specific permission sets
- This determines which accounts users can access and what they can do in each

#### e) Application Access:
- AWS SSO also manages access to business applications, either through SAML 2.0 or pre-integrated connectors

### Differences from Traditional IdP:

#### a) Scope:
- Traditional IdP: Primarily focused on authentication and basic authorization
- AWS SSO: Combines authentication, fine-grained authorization, and access management for AWS and external apps

#### b) AWS Integration:
- Traditional IdP: Requires additional configuration to work with AWS, often using SAML federation
- AWS SSO: Natively integrated with AWS Organizations, simplifying multi-account management

#### c) Permission Management:
- Traditional IdP: Often requires separate tools or processes for managing permissions within AWS
- AWS SSO: Includes built-in permission management for AWS resources across multiple accounts

#### d) User Experience:
- Traditional IdP: May require separate logins or configurations for different AWS accounts or applications
- AWS SSO: Provides a single access portal for all assigned AWS accounts and integrated applications

#### e) Setup and Maintenance:
- Traditional IdP: Often requires more complex setup and ongoing maintenance, especially for multi-account AWS environments
- AWS SSO: Streamlined setup and maintenance, especially when used with AWS Organizations

#### f) Application Integration:
- Traditional IdP: Typically requires manual configuration for each application
- AWS SSO: Offers pre-integrated connectors for many common business applications, simplifying setup



