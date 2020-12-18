# Aggregating AWS Health events across accounts with organizational view<a name="aggregate-events"></a>

By default, you can use AWS Health to view the AWS Health events of a single AWS account\. If you use AWS Organizations, you can also view AWS Health events centrally across your organization\. This feature provides access to the same information as single account operations\. You can use filters to view events in specific AWS Regions, accounts, and services\. 

You can aggregate events to identify accounts in your organization that are affected by an operational event or get notified for security vulnerabilities\. You can then use this information to proactively manage and automate resource maintenance events across your organization\. Use this feature to stay informed of upcoming changes to AWS services that might require updates or code changes\. 

**Notes**  
Organizational events are available for 90 days before they're deleted\. This quota can't be increased\.
AWS Health doesn't record events that occurred in your organization before you enabled organizational view\. For example, if a member account \(111122223333\) in your organization received an event for Amazon Elastic Compute Cloud \(Amazon EC2\) before you enabled this feature, this event won't appear in your organizational view\. 

## Prerequisites<a name="prerequisites-organizational-view"></a>

Before you use organizational view, you must:
+ Be part of an organization with [all features](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html#feature-set-all) enabled\.
+ Sign in to the management account as an AWS Identity and Access Management \(IAM\) user or assume an IAM role\.

  You can also sign in as the root user \(not recommended\) in your organization's management account\. For more information, see [Lock away your AWS account root user access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials) in the *IAM User Guide*\.
+ If you sign in as an IAM user, use an IAM policy that grants access to the AWS Health and Organizations actions, such as the [AWSHealthFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSHealthFullAccess) policy\. For more information, see [AWS Health identity\-based policy examples](security_iam_id-based-policy-examples.md)\. 

**Topics**
+ [Organizational view \(console\)](enable-organizational-view-in-health-console.md)
+ [Organizational view \(CLI\)](enable-organizational-view-from-aws-command-line.md)