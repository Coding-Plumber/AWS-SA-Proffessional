# Security Token Service (STS)

- Generates temporary credentials when sts:AssumeRole* API is called

When we assume an IAM role, we call the sts:AssumeRole API
- STS provides temporary credentials similar to long-term access keys:
    - Includes a public access key
    - Includes a secret access key
    - Includes a session token
- Key differences from long-term access keys:
    - Temporary credentials expire
    - Don't permanently belong to the identity
    - Provide limited access
- Used to access AWS resources securely

Temporary credentials can be requested by:
- AWS identities (e.g., IAM users, IAM roles)
- External identities (e.g., federated users via Google, Facebook, etc.)

## Trust Policies on IAM Roles

- The trust policy on an IAM role specifies who can assume the role
- STS will fail if the trust policy does not allow the assuming identity
- If the trust policy allows it, STS generates temporary credentials
- These credentials can access AWS resources until expiration
- Access is authorized based on the permission policy attached to the role

## Components of STS Credentials

AccessKeyId:
- Unique identifier for the temporary credentials

Expiration:
- Date and time when the credentials expire

SecretAccessKey:
- Used to cryptographically sign requests to AWS

SessionToken:
- Unique token that must be passed with all requests using these credentials
