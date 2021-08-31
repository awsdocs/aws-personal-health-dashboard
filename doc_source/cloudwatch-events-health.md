# Monitoring AWS Health events with Amazon CloudWatch Events<a name="cloudwatch-events-health"></a>

You can use Amazon CloudWatch Events to detect and react to changes for AWS Health events\. Then, based on the rules that you create, CloudWatch Events invokes one or more target actions when an event matches the values that you specify in a rule\. Depending on the type of event, you can send notifications, capture event information, take corrective action, initiate events, or take other actions\. For example, you can use AWS Health to receive email notifications if you have AWS resources in your AWS account that are scheduled for updates, such as Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

**Notes**  
This topic uses the CloudWatch Events console to create a rule\. You can also use the Amazon EventBridge console to create a rule\. For more information, see [Creating a rule for an AWS service](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.
For both services, AWS Health delivers events on a best effort basis\. Events are not always guaranteed to be delivered to CloudWatch Events or EventBridge\.
You can create a CloudWatch Events rule to receive notifications for only your account\. You can't receive organizational events for other accounts in your AWS Organizations\. For more information, see [Aggregating AWS Health events across accounts with organizational view](aggregate-events.md)\.

You can choose the following types of targets when using CloudWatch Events as a part of your AWS Health workflow:
+ AWS Lambda functions
+ Amazon Kinesis Data Streams
+ Amazon Simple Queue Service \(Amazon SQS\) queues
+ Built\-in targets \(CloudWatch alarm actions\)
+ Amazon Simple Notification Service \(Amazon SNS\) topics

For example, you can use a Lambda function to pass a notification to a Slack channel when an AWS Health event occurs\. Or, you can use Lambda and CloudWatch Events to send custom text or SMS notifications with Amazon SNS when an AWS Health event occurs\.

For samples of automation and customized alerts that you can create in response to AWS Health events, see the [AWS Health Tools](https://github.com/aws/aws-health-tools) in GitHub\.

**Topics**
+ [About AWS Regions for AWS Health](#choosing-a-region)
+ [About public events for AWS Health](#about-public-events)
+ [Creating a CloudWatch Events rule for AWS Health](#creating-cloudwatch-events-rule-for-aws-health)
+ [Receiving AWS Health events with AWS Chatbot](#receive-health-events-with-aws-chatbot)
+ [Automating actions for Amazon EC2 instances](#automating-instance-actions)

## About AWS Regions for AWS Health<a name="choosing-a-region"></a>

You must create a CloudWatch Events rule for each Region that you want to receive AWS Health events\. If you don’t create a rule, you won’t receive events\. For example, to receive events from the US West \(Oregon\) Region, you must create a rule for this Region\.

Some AWS Health events are not Region\-specific and instead are global, such as events sent for AWS Identity and Access Management \(IAM\)\. To receive global events, you must create a rule for the US East \(N\. Virginia\) Region\.

To receive global events in the AWS GovCloud \(US\), you must create a rule in the AWS GovCloud \(US\-West\) Region\.

## About public events for AWS Health<a name="about-public-events"></a>

Only AWS Health events that are specific to your AWS account are delivered to CloudWatch Events\. For example, this can include events such as a required update to an Amazon EC2 instance and other scheduled change events that might affect your account and resources\. 

Currently, you can't use CloudWatch Events to return *public* events from the [Service Health Dashboard](https://status.aws.amazon.com)\. Events from the Service Health Dashboard provide public information about the Regional availability of a service\. These events aren't specific to AWS accounts, so they aren't delivered to CloudWatch Events\.

Instead, you can use the AWS Health console and the [DescribeEventDetails](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html) operation\. You can use either option to return information about an event, and then identify if it's a public event from the Service Health Dashboard or an account\-specific event that affects your account\.

You can use the following options to identify if an event is public or account\-specific:
+ In the AWS Personal Health Dashboard, choose the **Affected resources** tab on the **Event log** page\. Events with resources are specific to your account\. Events without resources are public and are not specific to your account\. For more information, see [Getting started with the AWS Personal Health Dashboard](getting-started-phd.md)\.
+ Use the AWS Health API to return the `eventScopeCode` parameter\. Events can have the `PUBLIC`, `ACCOUNT_SPECIFIC`, or `NONE` value\. For more information, see the [DescribeEventDetails](https://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEventDetails.html) operation in the *AWS Health API Reference*\. 

You can also use the AWS Health Service Health Dashboard notifier tool to get notified for public events\. For more information, see the [aws\-health\-tools](https://github.com/aws/aws-health-tools/tree/master/shd-notifier) on the GitHub website\. 

## Creating a CloudWatch Events rule for AWS Health<a name="creating-cloudwatch-events-rule-for-aws-health"></a>

You can create a CloudWatch Events rule to get notified for AWS Health events in your account\. Before you create event rules for AWS Health, you should do the following:
+ Familiarize yourself with events, rules, and targets in CloudWatch Events\. For more information, see [What Is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide* and [New CloudWatch Events – Track and Respond to Changes to Your AWS Resources](https://aws.amazon.com/blogs/aws/new-cloudwatch-events-track-and-respond-to-changes-to-your-aws-resources/)\.
+ Create the target or targets to use in your event rules\.

**To create a CloudWatch Events rule for AWS Health**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. To change the AWS Region, use the **Region selector** in the upper\-right corner of the page\. Choose a Region in which you want to track AWS Health events\.

1. In the navigation pane, under **Events**, choose **Rules**\.

1. Choose **Create rule**, and then under **Event Source**, for **Service Name**, choose **Health**\.

1. For **Event Type**, choose one of the following options\.<a name="choose-service-category"></a>
   + Choose **All Events** to create a rule that applies to all AWS services\. This rule monitors all events from AWS Health\. If you choose this option, you can't specify event type categories or event type codes\.
   + Choose **Specific Health events**, **Specific service\(s\)**, and then choose a service name from the list\. This example creates a rule that monitors events for **EC2** so that CloudWatch Events monitors only Amazon EC2 events\.

1. If you chose a specific service, choose one of the following options\.
   + Choose **Any event type category** to create a rule that applies to all event type categories\.
   + Choose **Specific event type category\(s\)** and then choose a value from the list\. This creates a rule that applies only to one event type category, such as **scheduledChange**\. 
**Tip**  
To monitor all AWS Health events for a specific service, we recommend that you choose **Any event type category** and **Any resource**\. This ensures that your rule monitors for any AWS Health events, including any new event type codes, for your specified service\. For an example rule, see [all Amazon EC2 events](#all-ec2-events-rule)\.
You can create a rule to monitor for more than one service or event type category\. To do so, you must manually update the event pattern for the rule\. For more information, see [Creating a rule for multiple services and categories](#create-rule-multiple-services-categories)\.

1. If you chose a specific service and event type category, choose one of the following options for event type codes\.
   + Choose **Any event type code** to create a rule that applies to all event type codes\.
   + Choose **Specific event type code\(s\)** and then choose one or more values from the list\. This creates a rule that applies only to specific event type codes\. For example, if you choose **`AWS_EC2_INSTANCE_STOP_SCHEDULED`** and **`AWS_EC2_INSTANCE_RETIREMENT_SCHEDULED`**, your rule applies only to these events when they occur in your account\.

1. Choose one of the following options for affected resources\.
   + Choose **Any resource** to create a rule that applies to all resources\.
   + Choose **Specific resource\(s\)** and enter the IDs of one or more resources\. For example, you might specify an Amazon EC2 instance ID such as `i-EXAMPLEa1b2c3de4` to monitor for events that affect only this resource\.

1. <a name="review-your-rule"></a>Review your rule setup so that it meets your event\-monitoring requirements\.

1. In the **Targets** area, choose **Add target\***\.

1. In the **Select target type** list, choose the type of target that you prepared to use with this rule, and then configure any additional options required by that type\.

1. Choose **Configure details**\.

1. On the **Configure rule details** page, enter a name and description for the rule, and then select the **State** check box to enable the rule as soon as it's created\.

1. Choose **Create rule**\.<a name="all-ec2-events-rule"></a>

**Example : Rule for all Amazon EC2 events**  
The following example creates a rule so that CloudWatch Events monitors for all Amazon EC2 events, including the event type categories, event codes, and resources\.  

![\[Screenshot of how to create a CloudWatch Events rule for all Amazon EC2 events only.\]](http://docs.aws.amazon.com/health/latest/ug/images/cloudwatch-events-rule-all-ec2-health-events.png)

**Example : Rule for specific Amazon EC2 events**  
The following example creates a rule so that CloudWatch Events monitors the following:  
+ The Amazon EC2 service
+ The **scheduledChange** event type category
+ The event type codes for `AWS_EC2_INSTANCE_TERMINATION_SCHEDULED` and `AWS_EC2_INSTANCE_RETIREMENT_SCHEDULED`
+ The instance with the `i-EXAMPLEa1b2c3de4` ID

![\[Screenshot of how to create a CloudWatch Events rule for specific Amazon EC2 events only.\]](http://docs.aws.amazon.com/health/latest/ug/images/cloudwatch-events-rule-all-ec2-specific-events-resource.png)

### Creating a rule for multiple services and categories<a name="create-rule-multiple-services-categories"></a>

The examples in the previous procedure show you how to create a rule for a single service and event type category\. You can also create a rule for multiple services and event type categories\. This means that you don't have to create a separate rule for each service and category that you want to monitor\. To do so, you must edit the event pattern and then enter your changes manually\.

You can use any of the following options\.

**To add services and categories for an existing rule**

1. In the CloudWatch Events console, on the **Rules** page, choose the rule name\.

1. In the upper\-right corner, choose **Actions**, and then choose **Edit**\.

1. For **Event Pattern**, enter your changes in the text field\.

1. Choose **Configure details**, and then choose **Update rule** to save your changes\.

**To add services and categories for a new rule**

1. Follow the procedure in [Creating a CloudWatch Events rule for AWS Health](#creating-cloudwatch-events-rule-for-aws-health) to [step 6](#choose-service-category)\.

1. Instead of choosing a single service or category from the lists, for **Event Pattern Preview**, choose **Edit**\.

1. Enter your changes in the text field and choose **Save**\. See the following [example pattern](#example-multiple-services-categories)\.

1. Review your event pattern and then follow the rest of the procedure in [Creating a CloudWatch Events rule for AWS Health](#creating-cloudwatch-events-rule-for-aws-health) to create your rule\.

**Use the API or AWS Command Line Interface \(AWS CLI\)**  
For a new or existing rule, use the [PutRule](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutRule.html) API operation or the `aws events put-rule` command to update the event pattern\. For an example AWS CLI command, see [put\-rule](https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html) in the *AWS CLI Command Reference*\.<a name="example-multiple-services-categories"></a>

**Example: Multiple services and event type categories**  
The following event pattern creates a rule to monitor events for the `issue`, `accountNotification`, and `scheduledChange` event type categories and for three AWS services\.  

```
{
  "detail": {
    "eventTypeCategory": [
      "issue",
      "accountNotification",
      "scheduledChange"
    ],
    "service": [
      "AUTOSCALING",
      "VPC",
      "EC2"
    ]
  },
  "detail-type": [
    "AWS Health Event"
  ],
  "source": [
    "aws.health"
  ]
}
```

## Receiving AWS Health events with AWS Chatbot<a name="receive-health-events-with-aws-chatbot"></a>

You can receive AWS Health events directly in your chat clients, such as Slack and Amazon Chime\. Use this event to identify recent AWS service issues that might affect your AWS applications and infrastructure\. Then, you can sign in to your [AWS Personal Health Dashboard](https://phd.aws.amazon.com/phd/home) to learn more about the update\. For example, if you're monitoring for the `AWS_EC2_INSTANCE_STOP_SCHEDULED` event type in your AWS account, the AWS Health event can appear directly to your Slack channel\.

### Prerequisites<a name="prerequisited-chat-bot"></a>

Before you get started, you must have the following:
+ A chat client configured with AWS Chatbot\. You can configure Amazon Chime and Slack\. For more information, see [Getting started with AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/getting-started.html) in the *AWS Chatbot Administrator Guide*\.
+ An Amazon SNS topic, that you created and subscribed to\. If you already have an SNS topic, you can use an existing one\. For more information, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\.

**To receive AWS Health events with AWS Chatbot**

1. Follow the procedure in [Creating a CloudWatch Events rule for AWS Health](#creating-cloudwatch-events-rule-for-aws-health)\.

   1. When you choose the target in [step 11](#choose-target), choose an SNS topic\. You will use this same SNS topic in the AWS Chatbot console\.

   1. Complete the rest of the procedure to create the rule\.

1. Navigate to the [AWS Chatbot console](https://console.aws.amazon.com/chatbot)\.

1. Choose your chat client, such as your Slack channel name, and then choose **Edit**\. 

1. In the **Notifications \- optional** section, for **Topics**, choose the same SNS topic that you specified\.

1. Choose **Save**\.

   When AWS Health sends an event to CloudWatch Events that matches your rule, the AWS Health event will appear in your chat client\. 

1. Choose the event name to see more information in your AWS Personal Health Dashboard\.

**Example : AWS Health events sent to Slack**  
The following is an example of two AWS Health events for Amazon EC2 and Amazon Simple Storage Service \(Amazon S3\) in the US East \(N\. Virginia\) Region that appear in the Slack channel\.  

![\[Screenshot of how two AWS Health events appear in a Slack channel.\]](http://docs.aws.amazon.com/health/latest/ug/images/slack-chat-notification-for-health-events.png)

## Automating actions for Amazon EC2 instances<a name="automating-instance-actions"></a>

You can automate actions to respond to scheduled events for your Amazon EC2 instances\. When AWS Health sends an event to your AWS account, your CloudWatch Events rule can then invoke targets, such as AWS Systems Manager Automation documents, to automate actions on your behalf\.

For example, when an Amazon EC2 instance retirement event is scheduled for an Amazon Elastic Block Store \(Amazon EBS\)\-backed EC2 instance, AWS Health will send the `AWS_EC2_PERSISTENT_INSTANCE_RETIREMENT_SCHEDULED` event type to your AWS Personal Health Dashboard\. When your rule detects this event type, you can automate the stop and start of the instance\. This way, you don't have to perform these actions manually\.

**Notes**  
This procedure uses the CloudWatch Events console to create a rule\. You can also use the EventBridge console to create a rule\. For more information, see [Creating a rule for an AWS service](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.
To automate an action for your Amazon EC2 instances, they must be managed by Systems Manager\.

For more information, see [Automating Amazon EC2 with EventBridge](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/automating_with_cloudwatch_events.html) in the *Amazon EC2 User Guide for Linux Instances*\.

### Prerequisites<a name="prerequisites-automation-ec2-instances"></a>

You must create an AWS Identity and Access Management \(IAM\) policy, create an IAM role, and update the trust policy before you can create a rule\.

#### Create an IAM policy<a name="create-iam-role-for-ssm-automation"></a>

Follow this procedure to create a customer managed policy for your role\. This policy gives the role permission to perform actions on your behalf\. This procedure uses the JSON policy editor in the IAM console\.

**To create an IAM policy**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Copy the following JSON and then replace the default JSON in the editor\.

   1. In the `Resource` parameter, for the Amazon Resource Name \(ARN\), enter your AWS account ID\.

   1. You can also replace the role name or use the default\. This example uses *AutomationCWRole*\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "ec2:StartInstances",
              "ec2:StopInstances",
              "ec2:DescribeInstanceStatus"
            ],
            "Resource": [
              "*"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "ssm:*"
            ],
            "Resource": [
              "*"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::123456789012:role/AutomationCWRole"
          }
        ]
      }
      ```

1. Choose **Next: Tags**\.

1. \(Optional\) You can use tags as key–value pairs to add metadata to the policy\.

1. Choose **Next: Review**\.

1. On the **Review policy** page, enter a **Name** such as *AutomationCWRolePolicy* and a **Description** \(optional\)\. 

1. Review the **Summary** page to see the permissions that the policy allows, and then choose **Create policy**\.

This policy defines the actions that the role can take\. For more information, see [Creating IAM policies \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) in the *IAM User Guide*\. 

#### Create an IAM role<a name="creating-an-iam-role-for-ssm-automation"></a>

After you create the policy, you must create an IAM role, and then attach the policy to that role\.

**To create a role for an AWS service**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\. 

1. Choose **EC2** for the service that you want to allow to assume this role\.

1. Choose **Next: Permissions**\.

1. Enter the policy name that you created, such as *AutomationCWRolePolicy*, and then select the check box next to the policy\.

1. Choose **Next: Tags**\.

1. \(Optional\) You can use tags as key–value pairs to add metadata to the role\.

1. Choose **Next: Review**\. 

1. For **Role name**, enter *AutomationCWRole*\. This name must be the same name that appears in the ARN of the IAM policy that you created earlier\.

1. \(Optional\) For **Role description**, enter a description for the role\.

1. Review the role and then choose **Create role**\.

For more information, see [Creating a role for an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\.

#### Update the trust policy<a name="modify-trust-policy"></a>

Follow this procedure to update the trust policy for the role that you created\. You must complete this procedure so that you can choose this role in the CloudWatch Events console\.

**To update the trust policy for the role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the list of roles in your AWS account, choose the name of the role that you created, such as *AutomationCWRole*\.

1. Choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. For **Policy Document**, copy the following JSON, and then replace the default policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "ssm.amazonaws.com",
                       "events.amazonaws.com"
                   ]
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

1. Choose **Update Trust Policy**\.

For more information, see [Modifying a role trust policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-managingrole_edit-trust-policy) in the *IAM User Guide*\. 

### Create a rule for CloudWatch Events<a name="create-rule-for-ssm-automation"></a>

Follow this procedure to create a rule in the CloudWatch Events console so that you can automate the stop and start of EC2 instances that are scheduled for retirement\.

**To create a rule for CloudWatch Events for Systems Manager automated actions**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, under **Events**, choose **Rules**\.

1. Choose **Create rule**, and then under **Event Source**, for **Service Name**, choose **Health**\.

1. For **Event Type**, choose **Specific Health events**\.

1. Choose **Specific service\(s\)** and then choose **EC2**\.

1. Choose **Specific event type category\(s\)** and then choose **scheduledChange**\. 

1. Choose **Specific event types code\(s\)** and then choose the event type code\. For example, for Amazon EC2 EBS\-backed instances, choose **`AWS_EC2_PERSISTENT_INSTANCE_RETIREMENT_SCHEDULED`**\. For Amazon EC2 instance store\-backed instances, choose **`AWS_EC2_INSTANCE_RETIREMENT_SCHEDULED`**\.

1. Choose **Any resource**\.

   Your **Event Pattern Preview** might look like the following example\.  
**Example**  

   ```
   {
     "source": [
       "aws.health"
     ],
     "detail-type": [
       "AWS Health Event"
     ],
     "detail": {
       "service": [
         "EC2"
       ],
       "eventTypeCategory": [
         "scheduledChange"
       ],
       "eventTypeCode": [
         "AWS_EC2_PERSISTENT_INSTANCE_RETIREMENT_SCHEDULED"
       ]
     }
   }
   ```

1. Add the Systems Manager Automation document target\. Under **Targets**, choose **Add target\***, and then choose **SSM Automation**\.

1. For **Document\***, choose **`AWS-RestartEC2Instance`**\.

1. Expand the **Configure automation parameters\(s\)** and then choose **Input Transformer**\.

1. For the **Input Path** field, enter `{"Instances":"$.resources"}`\.

1. For the second field, enter `{"InstanceId": <Instances>}`\.

1. Choose **Use existing role** and then choose your IAM role that you created, such as *AutomationCWRole*\.

   Your target should look like the following example\.  
![\[Screenshot of the "SSM Automation" example in the CloudWatch Events console.\]](http://docs.aws.amazon.com/health/latest/ug/images/automation-actions-ssm.png)
**Note**  
If you don't have an existing IAM role with the required EC2 and Systems Manager permissions and trusted relationship, your role won't appear in the list\. For more information, see [Prerequisites](#prerequisites-automation-ec2-instances)\.

1. Choose **Configure details**\.

1. Enter a name and description for your rule\. Keep the **State** option selected\.

1. Choose **Create rule**\.