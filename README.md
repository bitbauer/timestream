## Amazon Timestream with Telegraf and Grafana Demo
The purpose of this repository is to demo how to ingest time series data in this case Amazon EC2 instance metrics such as CPU, Memory, Disk into Amazon Timestream.

AWS CloudFormation template (main.yaml) will also deploy an Amazon EC2 instance with Grafana.

###  Launch the AWS CloudFormation Stack

Click on the **Launch Stack** button below to launch the CloudFormation Stack to set up the Amazon Timestream with Telegraf Sample in the region of your preference, by default this demo will be deployed in eu-west-1 (Ireland) region.

[![Launch CFN stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template)  

AllowedIP=0.0.0.0/0  
LatestAmiId=/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2  
PublicSubnet1CIDR=10.10.10.0/24  
PublicSubnet2CIDR=10.10.11.0/24  
ResourceName=telegraf-to-timestream  
VpcCIDR=10.10.0.0/16  

Provide a stack name eg **timestream-telegraf**.

You can launch the same stack using the AWS CLI. Here's an example:

```
aws cloudformation create-stack --stack-name timestream-telegraf \
   --template-body file://main.yaml \
   --capabilities CAPABILITY_NAMED_IAM \
   --region eu-west-1
```

### Accessing Grafana Dashboard

Once stack creation is completed, it will output the Application Load Balancer DNS Name under "Outputs" tab of your stack. Another way of accessing via CLI:

```
aws cloudformation describe-stacks --stack-name timestream-telegraf \
   --query "Stacks[0].Outputs[0].OutputValue" \
   --region eu-west-1
```

**Username:** ```admin```
**Password:** ```admin```

After logged in, under Dashboards, you will find a sample dashboard called "Amazon Timestream Dashboard".

###  Clean up
After completing your demo, delete AWS CloudFormation Stack using AWS Console or AWS CLI:
```
aws cloudformation delete-stack --stack-name timestream-telegraf  --region eu-west-1
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
