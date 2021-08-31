# Organizational view \(console\)<a name="enable-organizational-view-in-health-console"></a>

You can use the AWS Health console to get a centralized view for health events in your AWS organization\.

Organizational view is available in the AWS Health console for all AWS Support plans at no additional cost\.

**Note**  
If you want to allow users access to this feature in the management account, they must have permissions such as the [https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSHealthFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSHealthFullAccess) policy\. For more information, see [AWS Health identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

**Contents**
+ [Enabling organizational view \(console\)](#enable-organizational-view-console)
+ [Viewing organizational view events \(console\)](#viewing-organizational-view-events-console)
  + [Dashboard](#organizational-view-console-dashboard)
  + [Event log](#organizational-view-console-event-log)
+ [Viewing affected accounts and resources \(console\)](#view-accounts-and-resources)
+ [Disabling organizational view \(console\)](#disabling-organizational-view-console)

## Enabling organizational view \(console\)<a name="enable-organizational-view-console"></a>

You can enable organizational view from the AWS Health console\. You must sign in to the management account of your AWS organization\.

**To view the AWS Personal Health Dashboard for your organization**

1. Sign in to the AWS Management Console and open the AWS Personal Health Dashboard at [https://phd.aws.amazon.com/phd/home](https://phd.aws.amazon.com/phd/home)\. 

1. In the navigation pane, choose **Organizational view**, and then choose **Configurations**\.

1. On the **Enable organizational view** page, choose **Enable organizational view**\.  
![\[Screenshot of how to enable organizational view in the AWS Health console.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-enable.png)

1. \(Optional\) If you want to make changes to your AWS organizations, such as creating organizational units \(OUs\), choose **Manage AWS Organizations**\. 

   For more information, see [Getting started with AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_tutorials_basic.html) in the *AWS Organizations User Guide*\.

**Notes**  
Enabling this feature is an asynchronous process and takes time to complete\. Depending on the number of accounts in your organization, it can take several minutes to load the accounts\. You can leave and check the AWS Health console later\.
 If you have a Business or Enterprise support plan, you can call the [DescribeHealthServiceStatusForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeHealthServiceStatusForOrganization.html) API operation to check the status of the process\.
When you enable this feature, the `AWSServiceRoleForHealth_Organizations` service\-linked role with the `Health_OrganizationsServiceRolePolicy` policy is applied to the management account in the organization\. For more information, see [Using service\-linked roles for AWS Health](using-service-linked-roles.md)\.

## Viewing organizational view events \(console\)<a name="viewing-organizational-view-events-console"></a>

After you enable organizational view, AWS Health displays health events for all accounts in your organization\. 

When an account joins your organization, AWS Health automatically adds the account to organizational view\. When an account leaves your organization, new events from that account are no longer logged to organizational view\. However, existing events remain and you can still query them up to the 90\-day limit\. 

 AWS retains the policy data for the account for 90 days from the effective date of the administrator account closure\. At the end of the 90 day period, AWS permanently deletes all policy data for the account\. 
+  To retain findings for more than 90 days, you can archive the policies\. You can also use a custom action with an EventBridge rule to store the findings in an S3 bucket\. 
+  As long as AWS retains the policy data, when you reopen the closed account, AWS reassigns the account as the service administrator and recovers the service policy data for the account\. 
+  For more information, see [Closing an account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/close-account.html)\. 

**Important**  
 For customers in the AWS GovCloud \(US\) Regions:   
 Before closing your account, back up and then delete your policy data and other account resources\. You will no longer have access to them after you close the account\. 

**Note**  
When you enable this feature, the AWS Health console can display public events from the [Service Health Dashboard](https://status.aws.amazon.com) for the last 7 days\. These public events aren't specific to accounts in your organization\. Events from the Service Health Dashboard provide public information about the regional availability of AWS services\.

You can view organizational view events in the **Dashboard** or the **Event log** page\.

**Topics**
+ [Dashboard](#organizational-view-console-dashboard)
+ [Event log](#organizational-view-console-event-log)

### Dashboard<a name="organizational-view-console-dashboard"></a>

You can use the **Dashboard** page to view events that might affect your AWS infrastructure, such as changes to AWS services and resources that affect your organization\.

**To view organizational view events in the Dashboard page**

1. Sign in to the AWS Management Console and open the AWS Personal Health Dashboard at [https://phd.aws.amazon.com/phd/home](https://phd.aws.amazon.com/phd/home)\. 

1. In the navigation pane, choose **Organizational view**, and then choose **Dashboard** to view recent and upcoming events\.

1. You can choose the **Open issues**, **Scheduled changes**, or **Other notifications** tab, and then choose the event name\. 

1. On the **Details** tab, you can review the following information about the event:
   + Event name
   + Status
   + Region / Availability Zone
   + Affected accounts
   + Start time
   + End time
   + Category
   + Description

**Example : Dashboard page for organizational view**  
The following Amazon Relational Database Service \(Amazon RDS\) event appears in the **Dashboard** page for organizational view and affects one account in the organization\.  

![\[Screenshot of organizational view events in the Dashboard page of the AWS Health console.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-dashboard.png)

### Event log<a name="organizational-view-console-event-log"></a>

You can also use the **Event log** page to view AWS Health events for organizational view\. The column layout and behavior are similar to the **Dashboard** page, except that the **Event log** page includes additional columns and filter options, such as the **Event category**, **Status**, and **Start time**\.

**To view organizational view events in the Event log page**

1. Sign in to the AWS Management Console and open the AWS Personal Health Dashboard at [https://phd.aws.amazon.com/phd/home](https://phd.aws.amazon.com/phd/home)\. 

1. In the navigation pane, choose **Organizational view**, and then choose **Event log**\.

1. Under **Event log**, choose the event name\. You can review the following information about the event:
   + Event name
   + Status
   + Region / Availability Zone
   + Affected accounts
   + Start time
   + End time
   + Category
   + Description

**Example : Event log page for organizational view**  
The following example Amazon DynamoDB \(DynamoDB\) event appears in the **Event log** page and affects two accounts in the organization\.  

![\[Screenshot of organizational view events in the Event log page of the AWS Health console.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-event-log.png)

## Viewing affected accounts and resources \(console\)<a name="view-accounts-and-resources"></a>

In the event details for the **Dashboard** and **Event log** pages, you can view the accounts in your organization that are affected by the event and any related resources\. For example, if there's an upcoming event for Amazon Elastic Compute Cloud \(Amazon EC2\) instance maintenance, accounts in your organization that have Amazon EC2 instances can appear in the **Details** tab\. You can identify the specific resources and then contact the account owner\.

**To view affected accounts and resources**

1. In the **Dashboard** or ** Event log** page, choose an event that has a value for **Affected accounts**\.

1. Choose the **Affected accounts** tab\.

1. Choose **Show account details** to view the following information for the accounts:
   + Account ID
   + Account name
   + Primary email
   + Organizational unit \(OU\)  
![\[Screenshot of how to show account details for an event in organizational view.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-affected-accounts.png)

1. Expand the account to view the affected resources\.  
![\[Screenshot of how to show affected resources for an account in organizational view.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-affected-accounts-resources.png)

1.  If there are more than 10 resources, choose **View all resources** to view them\.

1. To filter by account ID for this specific event, do the following: 

   1. On the **Affected accounts** tab, choose **Add filter**, choose **Account ID**, and then enter the account ID\. You can only enter one account ID at a time\. 

   1. Choose **Apply**\. The account that you entered appears in the list\.  
![\[Screenshot of how to filter by account ID for an event in organizational view.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-filter-accounts.png)

## Disabling organizational view \(console\)<a name="disabling-organizational-view-console"></a>

If you don't want to aggregate events for your organization, you can turn off this feature from the management account\.

AWS Health stops aggregating events for all other accounts in your organization\. You can continue to view previous events from your organization until they're deleted\.

**To disable organizational view**

1. Sign in to the AWS Management Console and open the AWS Personal Health Dashboard at [https://phd.aws.amazon.com/phd/home](https://phd.aws.amazon.com/phd/home)\. 

1. In the navigation pane, choose **Organizational view**, and then choose **Configurations**\.

1. On the **Enable organizational view** page, choose **Disable organizational view**\.  
![\[Screenshot of how to disable organizational view in the AWS Health console.\]](http://docs.aws.amazon.com/health/latest/ug/images/organizational-view-aws-health-console-disable.png)

After you turn off this feature, AWS Health no longer aggregates events from your organization\. However, the service\-linked role remains in the management account until you delete it through the AWS Identity and Access Management \(IAM\) console, IAM API, or AWS Command Line Interface \(AWS CLI\)\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.