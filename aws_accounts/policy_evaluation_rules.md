# AWS IAM Policy Evaluation Logic

## Policy Types (in order of evaluation)
1. Organization Service Control Policies (SCPs)
2. Resource Policies
3. IAM Permission Boundaries
4. Session Policies
5. Identity Policies

## Multi-Account vs Single Account Policy Evaluation

### Single Account Policy Evaluation
Policies are evaluated in the following order (from highest to lowest precedence):

1. Explicit Deny
2. SCPs
3. Resource Policies
4. Permission Boundaries
5. Session Policies
6. Identity Policies

AWS first gathers all policies that apply to the access attempt.

![Evaluation Logic Diagram](../imgs/diff_eval_aws.png)

### Cross-Account Access Logic
When accessing resources in a different account:

1. The identity from the originating account must allow the action.
2. The resource in the target account must also allow the action.

In other words, both the identity-based policy in the originating account and the resource-based policy in the target account must permit the access.

864981744755

Bucket1URL
https://us-east-1.console.aws.amazon.com/s3/buckets/buckets-petpics1-rpzosun7vxui?region=us-east-1&tab=objects
-
-
Bucket2URL
https://us-east-1.console.aws.amazon.com/s3/buckets/buckets-petpics2-0wz6sw0n0hn6?region=us-east-1&tab=objects
-
-
Bucket3URL
https://us-east-1.console.aws.amazon.com/s3/buckets/buckets-petpics3-knast8wjgcpm?region=us-east-1&tab=objects
-
-
Role
buckets-CrossAccountS3Role-SIi0LJl1rYDe


054037112477
