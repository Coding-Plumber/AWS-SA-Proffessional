
# ON EC2
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
get the role name
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/REPLACE_ME


# Local Machine
## LINUX or MACOS

export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=
aws s3 ls
aws ec2 describe-instances --region us-east-1

## WINDOWS

SET AWS_ACCESS_KEY_ID=AKID
SET AWS_SECRET_ACCESS_KEY=SAK
SET AWS_SESSION_TOKEN=TOKEN 
aws s3 ls
aws ec2 describe-instances --region us-east-1


# ON EC2 (Test Invalidation)

aws ec2 describe-instances --region us-east-1
aws s3 ls
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
get the role name
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/REPLACE_ME

# ON EC2 (After Stop and Start)

aws ec2 describe-instances --region us-east-1
aws s3 ls



