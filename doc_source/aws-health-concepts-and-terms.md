# Concepts for AWS Health<a name="aws-health-concepts-and-terms"></a>

Learn about AWS Health concepts and understand how you can use the service to maintain the health of your applications, services, and resources in your AWS account\.

**Topics**
+ [AWS Health event](#aws-health-events)
+ [AWS Personal Health Dashboard](#personal-health-dashboard-term)
+ [Service Health Dashboard](#service-health-dashboard)
+ [Event type code](#event-type-code)
+ [Event type categories](#event-type-categories)
+ [Event status](#event-status)
+ [Affected entities](#affected-entities)
+ [AWS Health API](#aws-health-api)
+ [Organizational view](#organizational-view-concepts)

## AWS Health event<a name="aws-health-events"></a>

AWS Health events, also known as Health events, are notifications that AWS Health sends on behalf of other AWS services\. You can use these events to learn about upcoming or scheduled changes that might affect your account\. For example, AWS Health can send an event if AWS Identity and Access Management \(IAM\) plans to deprecate a managed policy or AWS Config plans to deprecate a managed rule\. AWS Health also sends events when there are service availability issues in an AWS Region\. You can review the event description to understand the issue, identify any affected resources, and take any recommended actions\.

There are two types of Health events:

**Contents**
+ [Account\-specific event](#account-specific-event)
+ [Public event](#public-event)

### Account\-specific event<a name="account-specific-event"></a>

Account\-specific events are local to either your AWS account or an account in your AWS organization\. For example, if there's an issue with an Amazon Elastic Compute Cloud \(Amazon EC2\) instance type in a Region that you use, AWS Health provides information about the event and the name of the affected resources\. 

You can find account\-specific events from your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home), the [AWS Health API](https://docs.aws.amazon.com/health/latest/APIReference/Welcome.html), or use [Amazon CloudWatch Events to receive notifications](cloudwatch-events-health.md)\.

### Public event<a name="public-event"></a>

Public events are reported service events that aren't specific to an account\. For example, if there's a service issue for Amazon Simple Storage Service \(Amazon S3\) in the US East \(Ohio\) Region, AWS Health provides information about the event, even if you don't use that service or have S3 buckets in that Region\. We recommend that you review public notifications before you take action on them\.

You can find public events from your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home) or the [Service Health Dashboard](https://status.aws.amazon.com/)\.

**Note**  
You can't use CloudWatch Events to return public events because they're not related to a specific account\. For more information, see [About public events for AWS Health](cloudwatch-events-health.md#about-public-events)\.

## AWS Personal Health Dashboard<a name="personal-health-dashboard-term"></a>

The AWS Personal Health Dashboard provides a complete view for events and communications about AWS services\. You can use the dashboard to find account\-specific events that might affect the health of your account and public service events that provide general awareness\. Sign in to the AWS Management Console to view your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home)\. 

The AWS Personal Health Dashboard presents information in two ways: 
+ A dashboard that shows recent and upcoming events organized by event type category
+ A full event log that shows all events from the past 90 days
**Note**  
If you enabled the organizational view feature, you can view events for all accounts in your AWS organization\.

For more information, see [Getting started with the AWS Personal Health Dashboard](getting-started-phd.md)\.

## Service Health Dashboard<a name="service-health-dashboard"></a>

The [Service Health Dashboard](https://status.aws.amazon.com/) shows public events, which are reported service issues for AWS that provide information about service availability\. The Service Health Dashboard only shows public events, which aren’t specific to any account\. 

Your AWS Personal Health Dashboard shows both *public* events and *account\-specific* events\. We recommend that you use the AWS Personal Health Dashboard to learn about events that might affect you directly, such as a deprecated resource, and to learn about events that provide general awareness, such as an upcoming maintenance issue for a service in a Region\.

## Event type code<a name="event-type-code"></a>

The event type codes shown in a Health event include the affected service and the type of event\. For example, if you receive a Health event that has the `AWS_EC2_SYSTEM_MAINTENANCE_EVENT` event type code, this means that the service is scheduling a maintenance event that might affect you\. Use this information to plan ahead or take action for your account\.

## Event type categories<a name="event-type-categories"></a>

All Health events have an associated event type category\. For some events, the event type category might appear in the event type code, such as the `AWS_RDS_MAINTENANCE_SCHEDULED` code\. In this example, the category is *scheduled*\. You can use this information to understand event categories at a high level\.

We recommend that you monitor all event type categories\. Note that each category appears for different types of events\. You can also use the [DescribeEventTypes](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventTypes.html) API operation to find the event type category\.

**Account notification**  
These events provide information about the administration or security of your accounts and services\. These events might be informative, or they might require urgent action from you\. We recommend that you pay attention for these types of events and review all recommended actions\.  
The following are example event type codes for account notifications:  
+ `AWS_S3_OPEN_ACCESS_BUCKET_NOTIFICATION` – You have an Amazon S3 bucket that might allow public access\.
+ `AWS_BILLING_SUSPENSION_NOTICE` – Your account has outstanding charges and has been suspended, or you deactivated your account\.
+ `AWS_WORKSPACES_OPERATIONAL_NOTIFICATION` – There’s a service issue for Amazon WorkSpaces\.

**Issue**  
These events are unexpected events that affect AWS services or resources\. Common events in this category include communications about operational issues that are causing service degradation, or localized resource\-level issues for your awareness\.  
The following are example event type codes for issues:  
+ `AWS_EC2_OPERATIONAL_ISSUE` – An operational issue for a service, such as delays in using a service\.
+ `AWS_EC2_API_ISSUE` – An operational issue for a service's API, such as increased latency for an API operation\.
+ `AWS_EBS_VOLUME_ATTACHMENT_ISSUE` – A localized resource\-level issue that might affect your Amazon Elastic Block Store \(Amazon EBS\) resources\.
+ `AWS_ABUSE_PII_CONTENT_REMOVAL_REPORT` – This event means that your account might be suspended if you don't take action\.

**Scheduled change**  
These events provide information about upcoming changes to your services and resources\. Some events might recommend that you take action to avoid service disruptions, while others will occur automatically without any action on your part\. Your resource might be temporarily unavailable during the scheduled change activity\. All events in this category are account\-specific events\.  
The following are example event type codes for scheduled changes:  
+ `AWS_EC2_SYSTEM_REBOOT_MAINTENANCE_SCHEDULED` – An Amazon EC2 instance requires a reboot\.
+ `AWS_SAGEMAKER_SCHEDULED_MAINTENANCE` – SageMaker requires a maintenance event, such as fixing a service issue\.
If you use the AWS Health API or the AWS Command Line Interface \(AWS CLI\) to return event details, the `Event` object contains the `eventScopeCode` field with the `ACCOUNT_SPECIFIC` value\. For more information, see the [AWS Health API Reference](https://docs.aws.amazon.com/health/latest/APIReference/)\.

## Event status<a name="event-status"></a>

The event status tells you if the Health event is open, closed, or upcoming\. You can view Health events in the AWS Personal Health Dashboard or the AWS Health API for up to 90 days\.

## Affected entities<a name="affected-entities"></a>

Affected entities are AWS resources that might be affected by the event\. For example, if you receive a scheduled event for Amazon EC2 maintenance for a specific instance type that you're using in your account, you can use the Health event to determine the ID of the affected instances\. Use this information to address any potential service issue, such as creating or deprecating resources\.

## AWS Health API<a name="aws-health-api"></a>

You can use the AWS Health API to programmatically access the information that appears in the [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home), such as the following:
+ Get information about events that might affect your AWS services and resources
+ Enable or disable the organizational view feature for your AWS organization
+ Filter your events by specific services, event type categories, and event type codes

For more information, see the [AWS Health API Reference](https://docs.aws.amazon.com/health/latest/APIReference/Welcome.html)\. 

**Note**  
You must have a Business or Enterprise Support plan from [AWS Support](http://aws.amazon.com/premiumsupport/) to use the AWS Health API\. If you call the AWS Health API from an account that doesn't have a Business or Enterprise Support plan, you receive a `SubscriptionRequiredException` error\. 

## Organizational view<a name="organizational-view-concepts"></a>

You can use this feature to aggregate all health events for AWS accounts in your AWS Organizations into a single view in the AWS Personal Health Dashboard\. You can then sign in to the management account of your organization or use the AWS Health API to view all events that might affect the different accounts and resources\. You can enable this feature from the AWS Health console or API\. For more information, see [Aggregating AWS Health events across accounts with organizational view](aggregate-events.md)\.