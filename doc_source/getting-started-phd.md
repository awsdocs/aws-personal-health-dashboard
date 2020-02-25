# Getting Started with the AWS Personal Health Dashboard<a name="getting-started-phd"></a>

The AWS Personal Health Dashboard provides information about AWS Health events that can affect your account\. The information is presented in two ways: a dashboard that shows recent and upcoming events organized by category, and a full event log that shows all events from the past 90 days\.

## Dashboard Landing Page<a name="dashboard"></a>

The Personal Health Dashboard organizes issues in three groups: open issues, scheduled changes, and other notifications\. By default, the open issues and other notifications are restricted to issues whose start time is within the last seven days\. The scheduled changes group contains items that are ongoing or upcoming\.

When you select an event in the dashboard list, the Event Details pane appears with information about the event and resources that are affected by the event\. For more information, see [Event Details Pane](#event-details)\.

You can filter the items that appear in any group by choosing options from the filter list\. For example, you can narrow the results by Availability Zone, Region, event end time or last update time, AWS service, and others\.

To see all the events that apply to your account, rather than the recent ones that appear in the dashboard, choose the **See all issues** link above the list to display the [Event Log](#event-log)\.

To be notified or take an automatic action when events change, you can set up rules by using Amazon CloudWatch Events\. Choose the **Set up notifications with CloudWatch Events** link to go to the CloudWatch Events console\. For more information, see [Monitoring AWS Health Events with Amazon CloudWatch Events](cloudwatch-events-health.md)\.

## Event Log<a name="event-log"></a>

The Event log page of the Personal Health Dashboard displays all the AWS Health events that apply to your account\. The column layout and behavior is similar to the Dashboard, but the log page includes additional columns and filter options for **Status** and **Event category**\.

When you select an event in the Event log list, the Event Details pane appears with information about the event and resources that are affected by the event\. For more information, see [Event Details Pane](#event-details)\.

You can filter the items by choosing options from the filter list\. For example, you can narrow the results by status \(closed, open, or upcoming\), event category \(issue, notification, or scheduled change\), Availability Zone, Region, event end time or last update time, AWS service, and others\.

## Event Details Pane<a name="event-details"></a>

The Event details pane has two tabs\. The **Details** tab displays a text description of the event and relevant data about the event: the event name, status, Region and Availability Zone, start time, end time, and category\. The **Affected resources** tab displays information about any AWS resources that are affected by the event: 
+ The resource ID \(for example, an Amazon EBS volume ID such as `vol-a1b2c34f`\) or Amazon Resource Name \(ARN\), if available or relevant\.

You can filter the items that appear in the resources list by choosing options from the filter list\. You can narrow the results by resource ID or ARN\.

## Alerts for AWS Health Events<a name="alert-bell"></a>

The Personal Health Dashboard includes a bell icon in the console navigation bar with an Alerts drop\-down menu, which can display the number of recent AWS Health events that appear on the dashboard in each category\. This bell icon appears on several AWS consoles, including Amazon EC2, Amazon RDS, IAM, and AWS Trusted Advisor\. You can use this feature to see at a glance whether your account is affected by any recent events, and you can choose one of the menu items to go directly to the Personal Health Dashboard for more information\.