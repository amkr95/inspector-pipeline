# inspector-pipeline
CloudFormation stack that can be used to setup the AWS Inspector service, run Inspector scans according to a CloudWatch event schedule, and send the findings to an SNS queue.

This CloudFormation stack will create a couple of Lambda functions within your account; one to setup (if it hasn't already been setup) and run Inspector, and a second one to pull Inspector findings and send them to an SNS queue.

## Installation
From the AWS console, go to CloudFormation, then click on Create a New Stack.  Under Choose a Template, select Upload a Template to Amazon S3; you can then upload the ```aws-inspector-setup-and-pipeline.json``` file.

### Parameters

#### StackName
Enter in a name for the CloudFormation stack.  Note that the Lambda functions will be prepended by this StackName

#### AssessmentTemplateName
What to call the Inspector Assessment Template.  The Lambda functions will key off this name.

#### AssessmentTargetName
What to call the Inspector Assessment Target.  The Lambda functions will key off this name.

#### InspectorRunCronSchedule
The frequency with which you want to run AWS Inspector scans.  This is field is a CloudWatch Event schedule expression, which will ultimately trigger the ```InspectorRunner``` Lambda function.  See the [AWS Documentation](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) for more info/options on how to configure this field.

#### PushInspectorFindingsToSNSCronSchedule
The frequency with which you want to push findings from Inspector to the SNS topic.  This is field is a CloudWatch Event schedule expression, which will ultimately trigger the Lambda ```InspectorToSns``` Lambda function.  See the [AWS Documentation](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) for more info/options on how to configure this field.

#### SnsTopicToPushInspectorFindings
The SNS topic that you want to push your Inspector findings into.  You should change this value as the default Account ID is an invalid value.  You may also need to change the region to match where your SNS topic was provisioned.

#### EC2Tag fields
You should change these values to match your tagging convention.  Inspector can be setup to only run against machines that match a tag value.  For example, if you only wanted to run Inspector against machines whose ```Environment``` tag matches the value ```Production```, you can set that up by only specifying one EC2TagKey and EC2TagValue and leaving the other choices blank.
