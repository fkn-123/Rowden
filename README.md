Introduction

This repo contains the Terraform and Lambda function (as python code) to look for and shutdown EC2 instances that are tagged with the name Rowden.

1.Python Code For AWS Lambda (uses python 3.8)

Limitations

The code will only stop EC2 instances, it has not been set up to restart. This can easily be incorporated if required. 

The python routine is called stop_ec2.py

The python code was obtained from ChatGPT so I have included some explanations of selected lines in the code output below. 

https://www.online-ide.com/online_python_syntax_checker

import boto3
boto3 is the name of the python SDK for AWS

instances_to_stop = []
This creates an initial null list of instances to be stopped for every time the code is executed

response = ec2.describe_instances()
Obtain list of available attributes for all EC2 instances

for tag in instance.get('Tags', []):
This checks for tags on each EC2 instance. EC2 instances don’t need to be tagged, as it looks like it handles cases where instances don’t have any tags. Any instance that is not tagged will simply be looped over without error. However, it is recommended to tag everything in AWS as this helps with resource management (as it does here) as well as cost management. Tags can easily be applied through Terraform even retrospectively to existing resources. 

if tag['Key'] == 'Name' and 'Rowden' in tag['Value']:
Look for tags that contain the strings Name and Rowden

'statusCode': 200, 
Indicates normal successful completion of Lambda code (http response) after routine has looped through all instances which match and do not match the target string. 


2. Terraform code to launch lambda

Limitations

Due to the security group definition, the code will only work with Terraform 0.12 and later



![image](https://github.com/fkn-123/Rowden/assets/116656493/671d3539-1407-42cd-8cff-f1d9b99ad8e9)
