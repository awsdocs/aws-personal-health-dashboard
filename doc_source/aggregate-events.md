# Aggregating AWS Health Events across Accounts with Organizational View<a name="aggregate-events"></a>

AWS Health, by default, allows you to view the AWS Health events of a single account\. If you use AWS Organizations, you can also use Organizational View APIs to view AWS Health centrally across your organization\. You can use filters to view events in specific Regions, accounts, and services\. You can use Organizational View at no extra cost\.

You can aggregate AWS Health events in real\-time using Organizational View to identify accounts in your organization that are affected by an operational event\. You can also proactively manage and automate resource maintenance events across your organization\. By aggregating AWS Health events, you can be notified of security vulnerabilities and abuse issues, and you can stay informed of upcoming changes to AWS services that might require updates to code or policy\. As is standard for AWS Health, these organizational events are purged after 90 days\.

Before you start using the Organizational View APIs, you must:
+ Be part of an organization with [all features](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html#feature-set-all) enabled\.
+ Have a [Business](https://aws.amazon.com/premiumsupport/plans/business/) or [Enterprise](https://aws.amazon.com/premiumsupport/plans/enterprise/) support plan\.
+ Sign in as an IAM user, assume an IAM role, or sign in as the root user \(not recommended\) in your organization's master account\.
+ Be sure that the master account IAM policy grants [access to AWS Health API](https://docs.aws.amazon.com/health/latest/ug/controlling-access.html) Organizational View operations\.

To enable AWS Health to work with AWS Organizations, you call the [EnableHealthServiceAccessForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_EnableHealthServiceAccessForOrganization.html) API\. When you enable this feature, a [Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) \(SLR\) is applied to the master account in the organization 

**Note**  
Enabling this feature is an asynchronous process, and takes time to complete\. You can call the [DescribeHealthServiceStatusForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeHealthServiceStatusForOrganization.html) operation to see the status of the process\.

When you enable this feature, events that affect accounts in the organization begin accruing in the Organizational View from that point forward\. After 90 days, events are purged from the Organizational View\.

When an account joins your organization, that account is added to the Organizational View, and events are logged from that point forward\. When an account leaves your organization, new events are no longer logged to the Organizational View\. However, existing log entries remain and can still be queried, subject to the 90\-day limit of log entries\.

Organizational View provides access to the same information as single account operations —you can see the event metadata, descriptions, and so on—all in real\-time\. You can ingest these events into an in\-house ticketing system, dashboard, or monitoring service of your choice\. There are seven API operations to support the Organizational View capability:
+ [DescribeEventsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventsForOrganization.html) returns summary information about events across the AWS Organizations, meeting the specified filter criteria\.
+ [DescribeAffectedAccountsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedAccountsForOrganization.html) returns a list of AWS accounts in the AWS Organizations that are affected by the provided event\.
+ [DescribeEventDetailsForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetailsForOrganization.html) returns detailed information about one or more specified events for one or more accounts in AWS Organizations\.
+ [DescribeAffectedEntitiesForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeAffectedEntitiesForOrganization.html) returns a list of entities that have been affected by one or more events for one or more accounts in an AWS Organizations, based on the filter criteria\.

The following operations are used to enable or disable AWS Health from working with AWS Organizations: 
+ [EnableHealthServiceAccessForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_EnableHealthServiceAccessForOrganization.html) operation grants the AWS Health service permission to interact with AWS Organizations on the customer’s behalf and applies a Service Linked Role to the master account in the organization\.
+ [DisableHealthServiceAccessForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DisableHealthServiceAccessForOrganization.html) operation revokes permission for the AWS Health service to interact with AWS Organizations on the customer's behalf\.
+ [DescribeHealthServiceStatusForOrganization](https://docs.aws.amazon.com/health/latest/APIReference/API_DisableHealthServiceAccessForOrganization.html) operation provides status information on enabling or disabling AWS Health to work with your organization\.

 If Organizational View is disabled, then Health no longer aggregates events from your organization into the Organizational View\. However, the SLR remains on the master account until you [manually remove it](/IAM/latest/UserGuide/using-service-linked-roles.html) through the IAM console, IAM API, or AWS Command Line Interface \(AWS CLI\)\.
