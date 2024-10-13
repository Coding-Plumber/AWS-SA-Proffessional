# How to Revoke Temporary IAM Credentials

## Understanding IAM Role Temporary Credentials

- IAM roles can be assumed by multiple identities
- All assuming identities receive the same permissions
- Temporary credentials cannot be directly cancelled (they expire naturally)
- All role assumptions inherit the role's permission policy

## The Challenge with Leaked Credentials

Temporary credentials can last for hours. What if they were leaked during this time?

- Changing or deleting the role impacts all identities assuming that role
- We need a way to revoke credentials for a specific assumer without affecting others

### Example Scenario

Imagine three people have assumed a role:
1. Bob
2. Mary
3. Keith

They all assume the role and receive temporary credentials with identical permissions. However, Keith's credentials are compromised - a bad actor now has access to resources using Keith's temporary credentials.

If we change the role's permissions, it affects Bob and Mary as well. We need a targeted solution.

## Solution: Revoking Current Temporary Credentials

One effective action is to revoke all current temporary credentials and force users to obtain new ones. Here's how:

1. Update the role's policy to include an AWSRevokeOlderSessions inline policy
2. This policy will DENY access for any sessions older than 'NOW'

### Implementation

Add the following inline policy to the IAM role:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "DateLessThan": {"aws:TokenIssueTime": "${aws:CurrentTime}"}
      }
    }
  ]
}

### Effects of this Policy

- All existing temporary credentials become invalid immediately
- Users (including Bob and Mary) will need to re-assume the role to get new credentials
- The compromised credentials (Keith's) will no longer work
- New assumptions of the role will generate valid credentials

### Benefits

- Targeted: Affects only the specific role, not all roles or users
- Immediate: Takes effect as soon as the policy is added
- Comprehensive: Covers all actions and resources the role could access
- Non-disruptive: Legitimate users can quickly regain access by re-assuming the role

Remember: This doesn't revoke the actual credentials - it makes them ineffective for accessing AWS resources. The temporary credentials will still expire at their original time.
