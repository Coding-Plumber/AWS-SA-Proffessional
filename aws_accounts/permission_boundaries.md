# IAM Permission Boundaries

## Purpose and Key Characteristics

- Boundaries restrict existing roles/users
- They define the maximum permissions an identity can receive
- Only impact identity permissions
- Don't influence permissions Allowed or Denied within resource policies
- Can be applied to IAM users or IAM roles
- Don't grant any access on their own

## Example: Bob - IAM user

Bob has 7 permissions allocated via policies (6 Allow, 1 Deny)

Visual Representation:
```
-----------------------------
|      Permission Boundary   |
| Allow-Allow-Deny-Allow     |  Allow-Allow-Allow
|                            |  |      |       |
|                            |    ------------------
|                            |  These 3 permissions are allocated to the
|                            |  identity but outside the boundary, so they have no effect.
-----------------------------
```

## Use Cases

1. **Temporary Permission Elevation**
   - Grant elevated permissions without changing the base role
   - Ensure users can't exceed a certain level of access
   - Example: Give Bob admin permissions with boundaries to prevent creation of IAM users beyond the scope of the boundaries

2. **Delegated User Management**
   - Allow creation of new IAM users with restricted permissions
   - Useful for team leads or project managers who need to create and manage users within specific limits

3. **Development and Testing Environments**
   - Set boundaries for developers to experiment safely
   - Prevent accidental changes to production resources

4. **Vendor or Contractor Access**
   - Limit third-party access to specific resources or actions
   - Easily revoke or modify access without changing multiple policies

## Important Notes

- Permission boundaries don't influence resource policies
- They define the maximum possible permissions, not the actual granted permissions
- Effective permissions are the intersection of identity-based policies, resource-based policies, and permission boundaries
