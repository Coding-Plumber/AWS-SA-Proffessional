## Cognito

- Provides authentication, authorization, and user management for web and mobile apps

### User Pools - Sign in and get a JSON Web Token (JWT)
- User directory management and profiles
- Sign-up and sign-in (customizable web UI)
- MFA and other security features

> **Important**: When we think of User Pools, imagine a database of users which can include external identities. Users sign in and get a JWT.

### Identity Pools - Allow you to offer access to temporary AWS credentials
- Unauthenticated identities - Guest Users
- Federated identities - SWAP Google, FB, Twitter, SAML 2.0, and User Pools for short-term AWS credentials to access AWS resources

### User Pools Process
1. User Pool has a sign-in, either via a configured user pool or a 3rd party sign-in (e.g., Google, FB)
2. If sign-in is successful, it is authenticated and passed a JWT to the application
3. The application accepts it if correct and gives access to services like Lambda and API Gateway
   (It does not give access to resources that need a role to be accessed)
4. The main services accessed this way are API Gateway and legacy Lambda with custom auth

### Identity Pools Process
1. Identity Pool accepts 3rd party IdP such as Google, FB, etc.
   (This needs to be configured in the Identity Pool, usually by downloading a file from the 3rd party IdP for validation)
2. Once validated, it sends these to Cognito
3. Depending on the validation level, it can give either an authenticated role or an unauthenticated role (guest) with different permissions
4. This then gives temporary AWS credentials which allow access to the services based on the given role

### Identity Pools and User Pool Combination
1. Use the User Pool for authentication
2. Pass the JWT to the Identity Pool
3. Identity Pool gives either authenticated or unauthenticated role
4. Provides temporary credentials for this role to access AWS services

### Exam Tips
- **User Pools**: Manage user sign-up and sign-in, either internally or using social login. Returns a JSON Web Token (JWT)
- **Identity Pools**: SWAP external identity tokens for AWS credentials (federation). External identity tokens can be direct (e.g., Google, FB, Amazon) or User Pool tokens
