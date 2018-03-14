# Monitoring AWS Health Events with Amazon CloudWatch Events<a name="cloudwatch-events-health"></a>

You can use Amazon CloudWatch Events to detect and react to changes in the status of AWS Personal Health Dashboard \(AWS Health\) events\. Then, based on rules you create, CloudWatch Events invokes one or more target actions when an event matches the values you specify in a rule\. Depending on the type of event, you might want to send notifications, capture event information, take corrective action, initiate events, or take other actions\. You can select the following types of targets when using CloudWatch Events as a part of your AWS Health workflow:

+ AWS Lambda functions

+  Kinesis streams

+ Amazon SQS queues

+ Built\-in targets \(CloudWatch alarm actions\)

+ Amazon SNS topics

The following are some use cases:

+ Use a Lambda function to pass a notification to a Slack channel when an event occurs\.

+ Send custom text or SMS notifications via Amazon SNS when an AWS Health event happens by using Lambda and CloudWatch Events\.

For samples of automation and customized alerts that you can create in response to AWS Health events, see the [AWS Health Tools](https://github.com/aws/aws-health-tools) in GitHub\.

**Note**  
Only those AWS Health events that are specific to your AWS account and resources are published to CloudWatch Events\. This includes events such as EBS volume lost, EC2 instance store drive performance degraded, and all the scheduled change events\. In contrast, [Service Health Dashboard](https://status.aws.amazon.com) events provide information about the regional availability of a service and are not specific to AWS accounts, so they are not published to CloudWatch Events\. These event types have the word "operational" in the title in the Personal Health Dashboard; for example, "SWF operational issue"\.

The remainder of this topic describes the basic procedure for creating a CloudWatch Events rule for AWS Health\. Before you create event rules for AWS Health, however, you should do the following:

+ Familiarize yourself with events, rules, and targets in CloudWatch Events\. For more information, see [What Is Amazon CloudWatch Events?](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) and [New CloudWatch Events – Track and Respond to Changes to Your AWS Resources](https://aws.amazon.com/blogs/aws/new-cloudwatch-events-track-and-respond-to-changes-to-your-aws-resources/)\.

+ Create the target or targets to use in your event rules\. 

**To create a CloudWatch Events rule for AWS Health:**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**\.

1. Choose **Create rule**, and then under **Event Source**, for **Service Name**, choose **Health**\.

1. Specify AWS services:

   + To make a rule that applies to all AWS services, for **Event Type**, choose **All Events**\. If you choose all events, you cannot choose event type categories or event type codes\.

   + To make a rule that applies to events for one service only, choose **Specific Health events**, choose **Specific service\(s\)**, and then choose a service name from the list\. For example, **EC2**\. Note: you cannot choose more than one service\.

1. Specify event type categories \(if you have selected a specific service\):

   + To make a rule that applies to all event type categories, choose **Any event type category**\.

   + To make a rule that applies to one event type category only, choose **Specific event type category\(s\)**, and then choose a value from the list\. For example, **scheduledChange**\. Note: you cannot choose more than one category\. 

1. Specify event type codes \(if you have selected a specific service and a specific event type category\):

   + To make a rule that applies to all event type codes, choose **Any event type code**\.

   + To make a rule that applies to one or more event type codes only, choose **Specific event type code\(s\)**, and then choose one or more values from the list\. For example, **AWS\_EC2\_PERSISTENT\_INSTANCE\_RETIREMENT\_SCHEDULED**\.

1. Specify affected resources:

   + To make a rule that applies to all resources, choose **Any resource**\.

   + To make a rule that applies to one or more resources only, choose **Specific resource\(s\)**, and then type the IDs of one or more resources\. For example, **i\-a1b2c34f**\.

1. Review your rule setup to make sure it meets your event\-monitoring requirements\.

1. In the **Targets** area, choose **Add target\***\.

1. In the **Select target type** list, choose the type of target you have prepared to use with this rule, and then configure any additional options required by that type\. 

1. Choose **Configure details**\.

1. On the **Configure rule details** page, type a name and description for the rule, and then choose the **State** box to enable the rule as soon as it is created\.

1. If you're satisfied with the rule, choose **Create rule**\.