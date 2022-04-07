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
Please replace regio with your accounts region and account-id with your account id.

### 3-Login as IAM user and testing

Now login as created IAM user and you are able to start,stop and restart instance with tag "website" project tag "wordpress" and env tag "prod".
When you try to start,stop and restart other instance you will get the below error message.

![image](https://user-images.githubusercontent.com/100775801/162282864-bcdeedc2-adc0-489f-8852-7fac133f1219.png)

## Conclusion

In this article we had discussed how a IAM user can start, stop and restart with specific tags. We have done that with the help of IAM policy.

### ⚙️ Connect with Me

<p align="center">
 <a href="https://www.instagram.com/itz__me_omkar/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/sanu-das-t-3722891b5"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 


	







