# Controlling Access to the AWS Personal Health Dashboard and AWS Health<a name="controlling-access"></a>

You can use IAM to create identities \(users, groups, or roles\), and then give those identities permissions to access the Personal Health Dashboard and AWS Health API\.

By default, IAM users do not have access to the Personal Health Dashboard or AWS Health\. You give users access to your account's AWS Health information by attaching IAM policies to a single user, a group of users, or a role\. For more information, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) and [Overview of IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PoliciesOverview.html)\.

After you create IAM users, you can give those users individual passwords\. Then, they can sign in to your account and view AWS Health information by using an account\-specific sign\-in page\. For more information, see [How Users Sign In to Your Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_how-users-sign-in.html)\.

**Important**  
An IAM user with permissions to view Personal Health Dashboard has read\-only access to health information across all AWS services on the account, which can include, but is not limited to, AWS resource IDs such as Amazon EC2 instance IDs, EC2 instance IP addresses, and general security notifications\. For example, if an IAM policy grants access only to Personal Health Dashboard and AWS Health API, then the user or role that the policy applies to can access all information posted about AWS services and related resources, even if other IAM policies do not allow that access\.

AWS Health offers two groups of APIs:
+ Individual Account
  + [DescribeEvents](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEvents.html)
  + [DescribeEventDetails](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html)
  + [DescribeAffectedEntities](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedEntities.html)
  + [DescribeEventTypes](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventTypes.html)
  + [DescribeEventAggregates](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventAggregates.html)
  + [DescribeEntityAggregates](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEntityAggregates.html)
+ Organizational
  + [DescribeEventsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventsForOrganization.html)
  + [DescribeAffectedAccountsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedAccountsForOrganization.html)
  + [DescribeEventDetailsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetailsForOrganization.html)
  + [DescribeAffectedEntitiesForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedEntitiesForOrganization.html)
  + [EnableHealthServiceAccessForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_EnableHealthServiceAccessForOrganization.html)
  + [DisableHealthServiceAccessForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DisableHealthServiceAccessForOrganization.html)
  + [DescribeHealthServiceStatusForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeHealthServiceStatusForOrganization.html)

**Note**  
Although the Personal Health Dashboard is available for all AWS accounts, the AWS Health API is available only to accounts with a Business or Enterprise support plan\. For more information see [AWS Support](https://aws.amazon.com/premiumsupport/)\.

## Individual Actions<a name="individual-actions"></a>

To allow access to the Personal Health Dashboard and AWS Health, set the `Action` element of an IAM policy to `health:Describe*`\. AWS Health supports access control to Events based on the `eventTypeCode` and service\. See [Resource\- and Action\-based Conditions](#resource-action-based-conditions)\. To allow access to all events, set the `Resource` element to `*`\.

**Note**  
Although the Personal Health Dashboard is available for all AWS accounts, the AWS Health API is available only to accounts with a Business or Enterprise support plan\. For more information, see [AWS Support](https://aws.amazon.com/premiumsupport/)\.

For example, this policy statement grants access to Personal Health Dashboard and AWS Health:

```
{
  "Version": "2012-10-17",
  "Statement": [
  {
    "Effect": "Allow",
    "Action": [
      "health:Describe*"
    ],
    "Resource": "*"
  }]
}
```

This policy statement denies access to Personal Health Dashboard and AWS Health:

```
{
  "Version": "2012-10-17",
  "Statement": [
  {
    "Effect": "Deny",
    "Action": [
      "health:*"
    ],
    "Resource": "*"
  }]
}
```

If the user or group that you want to give permissions to already has a policy, you can add the AWS Health\-specific policy statement illustrated here to that policy\.

## Organizational View<a name="organizational-view"></a>

 To allow access to the Organizational AWS Health APIs set the `Action` element of an IAM policy to also include the following:
+ `iam:CreateServiceLinkedRole`
+ `organizations:EnableAWSServiceAccess`
+ `organizations:DisableAWSServiceAccess`
+ `organizations:ListAccounts`

To understand the exact permission need per APIs, see the IAM service page for [AWS Health](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awshealthapisandnotifications.html#awshealthapisandnotifications-actions-as-permissions)\.

**Note**  
Only the credential of the master account for an organization can access these APIs\.  
For example, this policy statement grants access to all AWS Health Organizational APIs:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "organizations:EnableAWSServiceAccess",
                "organizations:DisableAWSServiceAccess"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "organizations:ServicePrincipal": "health.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "health:*",
                "organizations:ListAccounts"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/health.amazonaws.com/AWSServiceRoleForHealth*"
        }
    ]
}
```

This policy statement explicitly denies access to Organizational APIs but allows access to Account APIs:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "health:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "organizations:EnableAWSServiceAccess",
                "organizations:DisableAWSServiceAccess"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "organizations:ServicePrincipal": "health.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": [
                "organizations:ListAccounts"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/health.amazonaws.com/AWSServiceRoleForHealth*"
        }
    ]
}
```

## Resource\- and Action\-based Conditions<a name="resource-action-based-conditions"></a>

AWS Health supports [IAM conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) for [ `DescribeAffectedEntities`](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedEntities.html) and [https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html)\. This allows you to restrict Events vended by AWS Health API on a per user, group, or role basis\. You can achieve this by populating the conditions block of the IAM Policy or by setting the `Resource` element\. You can use [String Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_String) to restrict access based on certain Health Event fields\. The following fields are supported:
+ `eventTypeCode`
+ `service`

For example, this policy statement grants access to Personal Health Dashboard and AWS Health, but denies access to any Health Events relating to EC2:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "health:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "health:DescribeAffectedEntities",
                "health:DescribeEventDetails"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "health:service": "EC2"
                }
            }
        }
    ]
}
```

The following policy has the same effect, but makes use of the `Resource` element:

```
{
  "Version": "2012-10-17",
  "Statement": [
  {
    "Effect": "Allow",
    "Action": [
      "health:Describe*"
    ],
    "Resource": "*",
  },
  {
    "Effect": "Deny",
    "Action": [
      "health:DescribeEventDetails",
      "health:DescribeAffectedEntities"
    ],
    "Resource": "arn:aws:health:*::event/EC2/*/*",
  }]
}
```

This policy statement grants access to Personal Health Dashboard and AWS Health, but denies access to any Health Events with type `AWS_EC2_*`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "health:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "health:DescribeAffectedEntities",
                "health:DescribeEventDetails"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "health:eventTypeCode": "AWS_EC2_*"
                }
            }
        }
    ]
}
```

**Important**  
An **AccessDeniedException** is produced when a user attempts to access an event that is denied by the associated user, group, or role\. This applies only to [DescribeAffectedEntities](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedEntities.html) and [DescribeEventDetails](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html)\.