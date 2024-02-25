# aws-login-alerting

Occasionally there are privileged accounts that should have minimal logins and when a login event occurs, people should know. For example, a production identity management account or any production accounts in some environments. This template ignores IAM user access as the original use case used IAM user accounts for programmatic scans and alerting on those would be verbose.

TODOS:
 - [X] e-mail alert when federated login occurs
 - [ ] slack alert when federated login occurs
 - [ ] add exception list to logins

Some assets are deployed in the Home Region and others are outside the Home Region.

Home Region Assets:
 - EventBridge EventRule to search for string and forward matches to Lambda
 - Lambda function (and roles) to parse the event and deliver to SNS
 - SNS Topic & Subscription to deliver notification to end-users

Other Region Assets:
 - EventBridge EventRule to search for string and forward to Home Region EventBridge


To deploy in the Home Region where the processing and alerting occurs:

```
aws cloudformation deploy --stack-name "console-login-alerting" --template-file cfn-console-login-alerting.yaml --profile 795177604632 --region us-east-1 --parameter-overrides ParameterKey=SNSSubscriptions,ParameterValue="will@crofton.cloud" ParameterKey=HomeRegion,ParameterValue=us-east-1 ParameterKey=LambdaTimeout,ParameterValue=60 --capabilities CAPABILITY_NAMED_IAM
```

Then for all subsequent regions, iterate the value of `--region`

```
aws cloudformation deploy --stack-name "console-login-alerting" --template-file cfn-console-login-alerting.yaml --profile 795177604632 --region us-east-2 --parameter-overrides ParameterKey=SNSSubscriptions,ParameterValue="will@crofton.cloud" ParameterKey=HomeRegion,ParameterValue=us-east-1 ParameterKey=LambdaTimeout,ParameterValue=60 --capabilities CAPABILITY_NAMED_IAM

aws cloudformation deploy --stack-name "console-login-alerting" --template-file cfn-console-login-alerting.yaml --profile 795177604632 --region us-west-1 --parameter-overrides ParameterKey=SNSSubscriptions,ParameterValue="will@crofton.cloud" ParameterKey=HomeRegion,ParameterValue=us-east-1 ParameterKey=LambdaTimeout,ParameterValue=60 --capabilities CAPABILITY_NAMED_IAM

aws cloudformation deploy --stack-name "console-login-alerting" --template-file cfn-console-login-alerting.yaml --profile 795177604632 --region us-west-2 --parameter-overrides ParameterKey=SNSSubscriptions,ParameterValue="will@crofton.cloud" ParameterKey=HomeRegion,ParameterValue=us-east-1 ParameterKey=LambdaTimeout,ParameterValue=60 --capabilities CAPABILITY_NAMED_IAM
```

Notes for quick removal:

```
aws cloudformation delete-stack --stack-name console-login-alerting --region us-west-2
aws cloudformation delete-stack --stack-name console-login-alerting --region us-west-1
aws cloudformation delete-stack --stack-name console-login-alerting --region us-east-2
aws cloudformation delete-stack --stack-name console-login-alerting --region us-east-1
```
