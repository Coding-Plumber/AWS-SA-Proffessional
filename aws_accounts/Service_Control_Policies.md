# Service Control Policies (SCPs)

SCPs can be attached to organizations, OUs, or individual accounts. They inherit down the organization tree.

- If attached to root container, it affects the entire organization
- If attached to an OU, it affects all accounts involving the OU tree
- If management account has SCP attached (directly, via OU, or org), the management account is never affected
  - It's the only account which can't be controlled, so it's best not to manage resources in it

Key points:
- SCPs are account permission boundaries
- They limit what the account can do
- SCPs DON'T grant any permissions
- Can never restrict root user (always has 100% access), but restricting the account effectively restricts the root user

Examples:
- Only use certain size EC2 instances in account
- Restrict to specific regions only

## Allow List vs Deny List

Two ways to use SCPs: block or allow by default. Default is deny list.

When you enable SCPs, AWS applies a default policy called "FullAWSAccess" to the organization and all OUs. By default, they have no effect.

FullAWSAccess policy:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

- If "Effect" is omitted, it defaults to deny
- Anything explicitly allowed in an SCP is a service which can have access granted to identities in that account
- If deny, then it overrules
- If none, it's deny

We can have:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "ec2:*"
            ],
            "Resource": "*"
        }
    ]
}
```

This would deny everything except S3 all and EC2 all.

- Allow list architecture is easier to mismanage and forget to allow resources, and requires more admin work
- Usually recommend deny architecture because of lower overhead

## Identity Policies

Effective permissions in an account are the overlap between any identity policies and any applicable SCPs.

- Identity Policy allows this but it's beyond what the SCP allows
- SCP - This is allowed in the SCP but not granted in an identity policy. SCPs don't grant access to anything

## Identity Policies vs SCPs

Effective permissions in an account are the overlap between any identity policies and any applicable SCPs. Here's a table illustrating this relationship:

```
+----------------+------------------------+---------------------------+
| SCP Allows     | Identity Policy Grants | Effective Permissions     |
+----------------+------------------------+---------------------------+
| S3 Access      | S3 Access              | ✅ S3 Access Allowed      |
+----------------+------------------------+---------------------------+
| EC2 Access     | EC2 Access             | ✅ EC2 Access Allowed     |
+----------------+------------------------+---------------------------+
| RDS Access     | RDS Access             | ✅ RDS Access Allowed     |
+----------------+------------------------+---------------------------+
| Lambda Access  | ❌ (Not granted)        | ❌ No Lambda Access       |
+----------------+------------------------+---------------------------+
| ❌ (Not allowed)| DynamoDB Access        | ❌ No DynamoDB Access     |
+----------------+------------------------+---------------------------+
```

Explanation:
- The first three rows show policies that match in both SCP and Identity Policy, resulting in effective permissions.
- The fourth row shows a policy allowed by SCP but not granted by the Identity Policy, resulting in no access.
- The fifth row shows a policy granted by the Identity Policy but not allowed by the SCP, also resulting in no access.

Remember:
- SCPs set the outer boundaries of permissions but don't grant any access on their own.
- Identity Policies grant permissions, but they're only effective if they fall within the boundaries set by SCPs.
- The effective permissions are always the intersection of what SCPs allow and what Identity Policies grant.
