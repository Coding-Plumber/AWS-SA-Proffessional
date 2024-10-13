# Policy Interpretation Deep Dive

## Basic Policy Structure
Single or list of statements 
If we have statement with [] it has multiple statements 
Inside policy documents every single statement has an effect with either 'Allow' or 'Deny'

## Policy Evaluation Order
Deny, Allow, Deny 
In AWS it starts with nothing which is implicit default deny 

## Policy Evaluation Rules
Explicit Deny ALWAYS wins 
If none its Deny by default 
When evaluating policies it can be beneficial to check the Allow statements first 

## Example Policy
Effect: "Allow",
Action: [
  "S3:PutObject",
  "S3:GetObject",
]
Resource: "arn:aws:s3::holidaygifts/*"

This gives access to this resource (S3 bucket)

Tip - Check if the policy resource is for resource such as S3 bucket or sub resource such as objects inside buckets 

## Overriding Policies
If we also had 
Effect: "Deny", 
Action: "S3:GetObject",
Resource: "arn:aws:s3::holidaygifts/*"

If we just had this then the previous Allow would be overruled and would be denied. 

## Conditional Policies
Effect: "Deny",
Action: "S3:GetObject",
Resource: "arn:aws:s3::holidaygifts/*",
Condition: {
  "DateGreaterThan": {"aws:CurrentTime": "2024-10-01T00:00:00Z"}
}

Then the Deny only applies when the condition is met. 

## NotAction
We can also have NotAction instead of Action:
Sid: "DenyNonApprovedRegions",
Effect: "Deny", 
NotAction: [
  "cloudfront:*",
  "IAM:*",
  "route53:*"
]
Resource: "*"

NotAction matches anything that is NOT listed inside this component.

## Inverse Conditions
Condition: {
  StringEquals: {
    "aws:RequestedRegion": [
      "ap-southeast-2", 
      "eu-west-1"
    ] 
  }
}

In exam try to be hyper aware for Inverse conditions. i.e. StringNotEquals + NotAction

## Complex Policy Example
Sid: "DenyNonApprovedRegions",
Effect: "Deny", 
NotAction: [
  "cloudfront:*",
  "IAM:*",
  "route53:*"
]
Resource: "*"
Condition: {
  StringNotEquals: {
    "aws:RequestedRegion": [
      "ap-southeast-2", 
      "eu-west-1"
    ] 
  }
}

This policy blocks any services except those in the NotAction from working outside of these approved regions.

## Wildcard Usage
* - All resources 
* is needed on a few policy actions including 
S3:ListAllMyBuckets 
S3:GetBucketLocation 

## Conditional Access Example
Effect: "Allow", 
Action: "S3:ListBucket", 
Resource: "arn:aws:s3::cl-animal-rescue",
Condition: {
  "StringLike": {
    S3:prefix: [
      "",
      "home/",
      "home/${aws:username}/*"
    ]
  }
}

This gives access to your own area inside the S3.

## Exam Hints 
Explicit Deny always overrules 
If no explicit Allow, default Deny 
If statement with no allow, needs to be used in conjunction with another statement 
Look out for any 'Not' items i.e. StringNotEquals or NotAction

