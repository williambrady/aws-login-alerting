# aws-login-alerting

Occasionally there are privileged accounts that should have minimal logins and when a login event occurs, people should know. For example, a production identity management account or any production accounts in some environments. This template ignores IAM user access as the original use case used IAM user accounts for programmatic scans and alerting on those would be verbose.

TODOS:
 - [X] e-mail alert when federated login occurs
 - [ ] slack alert when federated login occurs
 - [ ] add exception list to logins


Example:

```
aws cloudformation create-stack --stack-name aws-login-alerting --template-body file://aws-login-alerting.yaml --parameters ParameterKey=SNSSubscriptions,ParameterValue="teamDistributionList@example.org" ParameterKey=SNSTopicName,ParameterValue=federated-login-alerting ParameterKey=LambdaTimeout,ParameterValue=60 --capabilities CAPABILITY_NAMED_IAM --profile 795177604632 --region us-east-1
```
