# Logging AWS Health API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

AWS Health is integrated with CloudTrail, a service that captures all of the AWS Health API calls and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the Personal Health Dashboard or from your code to the AWS Health APIs\. Using the information collected by CloudTrail, you can determine the request that was made to AWS Health, the source IP address of the request, who made the request, when it was made, and so on\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS Health Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, API calls to AWS Health are tracked in CloudTrail log files, along with other AWS service records\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All AWS Health actions *except* `DescribeEventAggregates` are logged by CloudTrail and are documented in the [AWS Health API Reference](http://docs.aws.amazon.com/health/latest/APIReference/)\. For example, calls to the `DescribeEvents`, `DescribeEventDetails`, and `DescribeAffectedEntities` actions generate entries in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The identity information in the log entry helps you determine the following: 

+ Whether the request was made with root or IAM credentials

+ Whether the request was made with temporary security credentials for a role or federated user

+ Whether the request was made by another AWS service

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

You can store your log files in your Amazon S3 bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

If you want to be notified upon log file delivery, you can configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate AWS Health log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. 

For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

## Understanding AWS Health Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries\. Each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. Log entries are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following example shows a CloudTrail log entry that demonstrates the [DescribeEntityAggregates](http://docs.aws.amazon.com/health/latest/APIReference/API_DescribeEntityAggregates.html) action\.

```
{
   "Records": [
   {
   "eventVersion": "1.05",
   "userIdentity": {
      "type": "IAMUser",
      "principalId": "AIDACKCEVSQ6C2EXAMPLE",
      "arn": "arn:aws:iam::111122223333:user/JaneDoe",
      "accountId": "111122223333",
      "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "userName": "JaneDoe",
      "sessionContext": {"attributes": {
         "mfaAuthenticated": "false",
         "creationDate": "2016-11-21T07:06:15Z"
      }},
      "invokedBy": "AWS Internal"
    },
   "eventTime": "2016-11-21T07:06:28Z",
   "eventSource": "health.amazonaws.com",
   "eventName": "DescribeEntityAggregates",
   "awsRegion": "us-east-1",
   "sourceIPAddress": "203.0.113.0",
   "userAgent": "AWS Internal",
   "requestParameters": {"eventArns": ["arn:aws:health:us-east-1::event/EBS_LOST_VOLUME"]},
   "responseElements": null,
   "requestID": "05b299bc-afb9-11e6-8ef4-c34387f40bd4",
   "eventID": "e4deb9dc-dbc2-4bdb-8515-73e8abcbc29b",
   "eventType": "AwsApiCall",
   "recipientAccountId": "111122223333"
   }
   ],
   ...
}
```