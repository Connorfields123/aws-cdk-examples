
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## Features

### Observability and Monitoring
This stack implements AWS Well-Architected Framework best practices for end-to-end request tracing:

- **AWS X-Ray Tracing**: Enabled on API Gateway and Lambda for distributed tracing
- **DynamoDB Streams**: Captures all item changes for audit and observability
- **End-to-End Visibility**: Complete request path tracking from API Gateway → Lambda → DynamoDB

After deployment, you can view traces in the [AWS X-Ray Console](https://console.aws.amazon.com/xray/) to analyze request flows, identify bottlenecks, and troubleshoot issues.

### Security and Compliance Logging
This stack implements comprehensive logging for security monitoring and compliance:

- **API Gateway Access Logs**: Captures all API requests with caller identity, IP, timestamps, and response codes
- **Lambda Function Logs**: Structured JSON logging with 30-day retention policy
- **VPC Flow Logs**: Network traffic monitoring for security analysis (7-day retention)
- **DynamoDB Point-in-Time Recovery**: Continuous backups for data protection and audit trail
- **Structured Logging**: JSON-formatted logs for efficient querying with CloudWatch Logs Insights

## Security and Compliance

### CloudTrail Requirements
This application requires AWS CloudTrail to be enabled at the account level to log all API calls made to AWS services. Ensure CloudTrail is configured with:
- Multi-region trail enabled
- Log file validation enabled
- Logs stored in S3 with appropriate retention policy
- CloudWatch Logs integration for real-time monitoring

### Log Retention Policies
- **API Gateway Access Logs**: 30 days
- **Lambda Function Logs**: 30 days
- **VPC Flow Logs**: 7 days

Adjust retention periods in the CDK stack based on your compliance requirements.

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data 
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

### Viewing Traces
After making API requests, view the distributed traces in AWS X-Ray:
1. Navigate to the [AWS X-Ray Console](https://console.aws.amazon.com/xray/)
2. Select "Service Map" to see the complete architecture visualization
3. Select "Traces" to view individual request traces with timing details

### Viewing Logs
Access logs through CloudWatch Logs:
1. **API Gateway Access Logs**: CloudWatch Logs → Log group `/aws/apigateway/...`
2. **Lambda Function Logs**: CloudWatch Logs → Log group `/aws/lambda/apigw_handler`
3. **VPC Flow Logs**: CloudWatch Logs → Log group for VPC Flow Logs

Use CloudWatch Logs Insights to query structured JSON logs:
```
fields @timestamp, message, request_id, item_id
| filter level = "INFO"
| sort @timestamp desc
```

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
