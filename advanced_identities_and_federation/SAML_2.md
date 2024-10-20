## SAML 2.0 Identity Federation 

- Allows use of external identities to access AWS resources
- SAML stands for Security Assertion Markup Language 
- Open standard used by many Identity Providers (IdPs), e.g., Microsoft Active Directory Federation Services (ADFS)
- Enables indirect use of on-premises identities with AWS (console & CLI)
- Suitable for enterprises with:
  - SAML 2.0 compatible identity provider
  - Existing identity management team
  - Single source of truth for identities
  - More than 5,000 users

### Key Points:
- Uses IAM Roles & AWS temporary credentials (valid for up to 12 hours)
- Not compatible with social identity providers like Google, Facebook, or Twitter
- If exam mentions web identity federation, it's likely not referring to SAML 2.0

### How SAML 2.0 Works:

1. Application (e.g., Doggogram) requests access via the IdP 
2. The IdP (e.g., MS ADFS) authenticates the request and identifies available roles for the application
3. The IdP sends a SAML Assertion to the app, containing authenticated user information and roles/permissions
4. Application uses the SAML assertion to call AWS STS using AssumeRoleWithSAML API
5. IAM processes the request, assumes the appropriate IAM role based on the SAML assertion, and returns temporary security credentials
6. Application uses temporary credentials to access AWS resources

### Setup Requirements:
- Establish a trust relationship between IdP and IAM
- In IAM: Select the IdP and upload the metadata XML file provided by the IdP
- Configure the IdP to recognize AWS as a Service Provider

### Console Access Setup:
1. User accesses IdP's URL
2. IdP authenticates the user
3. IdP generates SAML assertion
4. User's browser posts the SAML assertion to AWS SAML endpoint
5. AWS verifies the SAML assertion and creates a sign-in token
6. AWS redirects the user to the AWS Management Console

Note: Steps 4-6 happen behind the scenes, transparent to the user.

