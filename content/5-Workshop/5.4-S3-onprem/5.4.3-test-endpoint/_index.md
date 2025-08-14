---
title : "Test the Interface Endpoint"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Get the regional DNS name of S3 interface endpoint
1. From the Amazon VPC menu, choose Endpoints.

2. Click the name of newly created endpoint: s3-interface-endpoint. Click details and save the regional DNS name of the endpoint (the first one) to your text-editor for later use. 

![dns name](/images/5-Workshop/5.4-S3-onprem/dns.png)


#### Connect to EC2 instance in "VPC On-prem"

1. Navigate to **Session manager** by typing "session manager" in the search box 

2. Click **Start Session**, and select the EC2 instance named **Test-Interface-Endpoint**. This EC2 instance is running in "VPC On-prem" and will be used to test connectivty to Amazon S3 through the Interface endpoint we just created. Session Manager will open a new browser tab with a shell prompt: **sh-4.2 $**

![Start session](/images/5-Workshop/5.4-S3-onprem/start-session.png)

3. Change to the ssm-user's home directory with command "cd ~"

4. Create a file named testfile2.xyz
```
fallocate -l 1G testfile2.xyz
```

![user](/images/5-Workshop/5.4-S3-onprem/cli1.png)


5. Copy file to the same S3 bucket we created in section 3.2

```
aws s3 cp --endpoint-url https://bucket.<Regional-DNS-Name> testfile2.xyz s3://<your-bucket-name>
``` 
+ This command requires the --endpoint-url parameter, because you need to use the endpoint-specific DNS name to access S3 using an Interface endpoint.
+ Do not include the leading ' * ' when copying/pasting the regional DNS name.
+ Provide your S3 bucket name created earlier

![copy file](/images/5-Workshop/5.4-S3-onprem/cli2.png)


Now the file has been added to your S3 bucket. Let check your S3 bucket in the next step.

#### Check Object in S3 bucket

1. Navigate to S3 console
2. Click Buckets
3. Click the name of your bucket and you will see testfile2.xyz has been added to your bucket

![check bucket](/images/5-Workshop/5.4-S3-onprem/check-bucket.png)




