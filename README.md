# IAM-policy-for-start-stop-and-restart-instance-based-on-their-tags

## Description

In this article we are going to write a IAM policy which give a IAM user to see all the instance in amazon console but only have privilage to start,stop and restart some instance with specific tags. We are going to do this on the base of tags that we provide while creating the instance.

Consider we have two instance with same name tag "website" and project tag "wordpress" and both of them have env tag which is different. First instance env tag value is "prod" and second instance env tag value is "dev". So, here we are going to provide a user see all the instance and have privilage to start, stop and restart instance which have name tag "website" project tag "wordpress" and env tag "prod".

## Table of Contents

1. Creating two instace with tags
2. Creating IAM user with custome IAM policy
3. Login as IAM user and testing

### 1-Creating two instace with tags

First we are going to create two instace. login into your AWS management console and search EC2 on the top navbar. Then select EC2 from the drop-down. Select the instance section and click create instance for creating the instance.
The first instance Name tag value is "website" project tag value is "wordpress" and env tag value is "prod".

~~~
Name = websever
project = wordpress
env = prod
~~~

Second instance Name tag value is "website" project tag value is "wordpress" and env tag value is "dev".
~~~
Name = websever
project = wordpress
env = dev
~~~

### 2-Creating IAM user with custome IAM policy
Now we are going to create a IAM policy that enables the IAM user to see the list of all the instances on the AWS console, but will only have privilege to stop, start and reboot the instance with tag value is "website" project tag value is "wordpress" and env tag value is "prod".
For creating the policy go to IAM console and select policy and click create policy. Then select JSON and add the Below IAM policy

~~~
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances"
            ],
            "Resource": "arn:aws:ec2:<region>:<account-id>:instance/*",
            "Condition": {
                "StringEquals": {
				    "ec2:ResourceTag/Name": "webserver",
                    "ec2:ResourceTag/project": "wordpress",
                    "ec2:ResourceTag/env": "prod"
                }
            }
        }
    ]
}
~~~
please replace <region>and <account-id> with your account region and accound id.
  
### Login as IAM user and testing




