# AWS-IAM-User-Setup
This guide outlines how to create an AWS IAM user, group, and custom policy to allow full access to **EC2**, **S3**, **RDS**, and **Route 53** services using the AWS CLI.

### AWS IAM User Setup with Full EC2, S3, RDS, and Route 53 Access

This guide outlines the step-by-step process of creating an AWS IAM user, group, and attaching a custom policy to allow full access to **EC2**, **S3**, **RDS**, and **Route 53** services using the AWS CLI.

## Prerequisites
- AWS CLI installed and configured with appropriate AWS credentials.
- Permissions to create IAM users, groups, and policies.

## Steps

### 1. Create a New IAM User

To create a new IAM user, run the following command:
```bash
aws iam create-user --user-name <UserName>
```

Replace <UserName> with the desired name of the user.
```bash
aws iam create-user --user-name test-user
```

To verify the creation of the user, use:
```bash
aws iam list-users
```

### 2. Create an IAM Group

Create a group to manage permissions for users.
```bash
aws iam create-group --group-name <GroupName>
```

Replace <GroupName> with the desired name of the group.
```bash
aws iam create-group --group-name developers
```

To verify the group creation:
```bash
aws iam list-groups
```
### 3. Create a Custom Policy for Full Access to EC2, S3, RDS

Create a file named policy.json with the following content:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "s3:*",
                "rds:*",
                "route53:*"
            ],
            "Resource": "*"
        }
    ]
}
```

### 4. Create and Attach the Policy to the Group

To create the policy in AWS, run:
```bash 
aws iam create-policy --policy-name FullAccessEC2S3RDSRoute53 --policy-document file://policy.json
```

After the policy is created, attach it to the IAM group:
```bash
aws iam attach-group-policy --group-name <GroupName> --policy-arn arn:aws:iam::<Account-ID>:policy/FullAccessEC2S3RDSRoute53
```
Replace <GroupName> with the name of your group and <Account-ID> with your AWS account ID.

To verify the policy has been attached:
```bash
aws iam list-attached-group-policies --group-name <GroupName>
```
### 5. Add the User to the Group

Now that the group has the correct permissions, add the user to the group:
```bash
aws iam add-user-to-group --user-name <UserName> --group-name <GroupName>
```
Replace <UserName> and <GroupName> accordingly.

To verify the user has been added to the group:
```bash
aws iam list-groups-for-user --user-name <UserName>
```

### 6. (Optional) Create Access Keys for Programmatic Access

If the user needs programmatic access (e.g., API access), you can create access keys:
```bash
aws iam create-access-key --user-name <UserName>
```
The command should return an access key ID and a secret access key, which can be used by the user to access AWS resources programmatically.

### 7. Clean Up (Optional)
If you want to remove the user, group, or policy later, use the following commands:

Remove the user from the group:
```bash
aws iam remove-user-from-group --user-name <UserName> --group-name <GroupName>
```

Detach the policy from the group:
```bash
aws iam detach-group-policy --group-name <GroupName> --policy-arn arn:aws:iam::<Account-ID>:policy/FullAccessEC2S3RDSRoute53
```
Delete the user:
```bash
aws iam delete-user --user-name <UserName>
```

Delete the group:

```bash
aws iam delete-group --group-name <GroupName>
```
Delete the policy:
```bash
aws iam delete-policy --policy-arn arn:aws:iam::<Account-ID>:policy/FullAccessEC2S3
```

Comeference of all the commands used:
```bash
# Create an IAM user
aws iam create-user --user-name test-user

# Create an IAM group
aws iam create-group --group-name developers

# Create a policy from policy.json
aws iam create-policy --policy-name FullAccessEC2S3RDSRoute53 --policy-document file://policy.json

# Attach the policy to the group
aws iam attach-group-policy --group-name developers --policy-arn arn:aws:iam::<Account-ID>:policy/FullAccessEC2S3RDSRoute53

# Add the user to the group
aws iam add-user-to-group --user-name test-user --group-name developers

# (Optional) Create access keys for the user
aws iam create-access-key --user-name test-user
```

Note:
This process creates a new user, adds them to a group, and grants full access to EC2, S3, RDS, and Route 53 via a custom policy.