# AWS inline Policy and Bucket Policy
***
<br />

****Sample representation and use cases of 2 different policies in AWS.****

<br />
<br />

***What is Policy in AWS?***
<br />
<br />
A policy is an object in AWS that, when associated with an entity or resource, defines their permissions.
We can control access in AWS resources by creating policies and attaching them to IAM identities or AWS resources.
Policies can be created based on the requirement of the AWS resource or user needed for the project we are working with.

Most policies are stored in AWS as **JSON** documents.

Here, we are describing 2 kind of policies. One for restricting privileges for a AWS user and the other for accessing an AWS resource, S3 bucket.
- ****IAM Policy****
- ****Bucket Policy****


<br />
<br />

#### IAM user inline Policy:

IAM policies define permissions for IAM user to access resources on AWS. 
For example, if a policy allows the GetUser action, then a user with that policy can get user information from the AWS Management Console, the AWS CLI, or the AWS API. 
When you create an IAM user, you can set up the user to allow console or programmatic access. 
The IAM user can sign in to the console using a user name and password. Or they can use access keys to work with the CLI or API.

Inline policies are policies that you create and manage and embed directly into a single user, group, or role.

Let's discuss a case of restricting an AWS resource, Route53 hosted zones for all zone entries except a particular hosted zone entry.
As already stated, this is an independant policy assigned for a particular user.
So the policy will be defined only under that user's "Inline" policy section.

For this case, the policy will be as follows:
```sh 
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:*"
            ],
            "Resource": "arn:aws:route53:::hostedzone/Z08712646EF6JSRNFSI8"
        },
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ListHostedZones",
                "route53:ListHostedZonesByName",
                "route53:GetHostedZoneCount"
            ],
            "Resource": "*"
        }
    ]
}
```
In this scenario, that user will have full access to the particular Hosted Zone with ID "Z005216210IY57VRKRQG7" and only have "Get" and "List" privileges on other Hosted zone entries existing in the AWS account.
The user will not be able to edit anything on other Hosted zones.
FYI, ARN is Amazon Resource Name format identifies resources in AWS. 
Hosted zone ID can be obtained from AWS Route53 console as shown below:<br />
![image](https://user-images.githubusercontent.com/117455666/210540301-89eeb52e-ac94-4a28-8e4f-aff7c7986e0d.png)



<br />
<br />

#### Bucket Policy:

Bucket policies are attached only to AWS resource, S3 buckets. 
S3 bucket policies specify what actions are allowed or denied for which principals on the bucket that the bucket policy is attached.

Here, I'm sharing 2 different examples for S3 bucket policy.

1)
Usually, public access on S3 buckets are disabled by default.
AWS recommends that you turn on Block all public access, but before applying any of these settings, ensure that your applications will work correctly without public access. If you require some level of public access to your buckets or objects, you can customize the individual settings below to suit your specific storage use cases.

I'm sharing the policy for a case of public read access to the objects under a particular S3 bucket.

Bucket policy should be defined under "Permissions" on that particular S3 bucket to which the policy will be applied.
The policy will be as follows:
```sh 
 {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::images-s3-website.haashdev.tech/*"
		}
	]
}
```
In this scenario, we will be able to see the objects from the S3 bucket named "images-s3-website.haashdev.tech" while accessing with the public URL available on the S3 bucket.
<br />
2)
Another example is for a case of restricting an S3 bucket for a particular IAM user.
```sh 
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::953912081854:user/john"
			},
			"Action": "s3:*",
			"Resource": [
				"arn:aws:s3:::bucket-john.haashdev.tech",
				"arn:aws:s3:::bucket-john.haashdev.tech/*"
			]
		}
	]
}
```
In this scenario, only the IAM user "john" will have access over the S3 bucket named "bucket-john.haashdev.tech".
IAM user IAM can be obtained from "IAM Users" console as shown in the image given below:
<img width="533" alt="johnarn" src="https://user-images.githubusercontent.com/117455666/210540425-17949cdf-411a-4d47-91fb-f62948f6c0ed.png">



Also, the ARN for an S3 bucket can be copied using the option shown below in AWS console:


![image](https://user-images.githubusercontent.com/117455666/210541259-c35d6ce8-cd64-421b-96ed-22fd5a9ab5dc.png)

<br />


Thank you.
