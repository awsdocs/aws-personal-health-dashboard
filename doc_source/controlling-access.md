# Controlling Access to the AWS Personal Health Dashboard and AWS Health<a name="controlling-access"></a>

You can use IAM to create identities \(users, groups, or roles\), and then give those identities permissions to access the Personal Health Dashboard and AWS Health API\.

By default, IAM users do not have access to the Personal Health Dashboard or AWS Health\. You give users access to your account's AWS Health information by attaching IAM policies to a single user, a group of users, or a role\. For more information, see [Identities \(Users, Groups, and Roles\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) and [Overview of IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/PoliciesOverview.html)\.

After you create IAM users, you can give those users individual passwords\. Then, they can sign in to your account and view AWS Health information by using an account\-specAn IAM user with permissions to view ific sign\-in page\. For more information, see [How Users Sign In to Your Account](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_how-users-sign-in.html)\.

**Important**  
An IAM user with permissions to view Personal Health Dashboard has read\-only access to health information across all AWS services on the account, which can include, but is not limited to, AWS resource IDs such as EC2 instance IDs, EC2 instance IP addresses, and general security notifications\. For example, if an IAM policy grants access only to Personal Health Dashboard and AWS Health API, the user or role that the policy applies to has access to all information posted about AWS services and related resources, even if other IAn IAM user with permissions AM policies do not allow that access\.

To allow access to the Personal Health Dashboard and AWS Health, set the `Action` element of an IAM policy to `health:Describe*`\. AWS Health does not provide resource\-level access, so the `Resource` element is always set to `*`\. 

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