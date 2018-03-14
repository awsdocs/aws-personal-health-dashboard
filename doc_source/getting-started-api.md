# Getting Started with the AWS Health API<a name="getting-started-api"></a>

The AWS Health API provides programmatic access to the AWS Health information that is presented in the Personal Health Dashboard\. You can use these API operations to get information about events that affect your AWS resources:

+ `DescribeEvents`: Summary information about events\.

+ `DescribeEventDetails`: Detailed information about one or more events\.

+ `DescribeAffectedEntities`: Information about AWS resources that are affected by one or more events\.

In addition, these operations provide information about event types and summary counts of events or affected entities:

+ `DescribeEventTypes`: Information about the kinds of events that AWS Health tracks\.

+ `DescribeEventAggregates`: A count of the number of events that meet specified criteria\.

+ `DescribeEntityAggregates`: A count of the number of affected entities that meet specified criteria\.

The Health API requires a Business or Enterprise support plan from AWS Support\. Calling the Health API from an account that does not have a Business or Enterprise support plan causes a `SubscriptionRequiredException`\.

For detailed descriptions of the AWS Health API, see the [AWS Health API Reference](http://docs.aws.amazon.com/health/latest/APIReference/)\.

Service Endpoint

The endpoint for the AWS Health API is: 

+ https://health\.us\-east\-1\.amazonaws\.com

The AWS Health service has only one endpoint, in the US East \(N\. Virginia\) Region, that provides event data for all regions\. You can filter query results to get information about events that affect one or more regions, but the query should always be addressed to the us\-east\-1 endpoint\. 

**Java Code Examples**  
The following section demonstrates using the AWS Health API with the AWS SDK for Java\.