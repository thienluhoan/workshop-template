---
title : "Clean up"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---
Congratulations on completing this workshop! 
In this workshop, you learned architecture patterns for accessing Amazon S3 without using the Public Internet. 
+ By creating a gateway endpoint, you enabled direct communication between EC2 resources and Amazon S3, without traversing an Internet Gateway. 
+ By creating an interface endpoint you extended S3 connectivity to resources running in your on-premises data center via AWS Site-to-Site VPN or Direct Connect. 

#### clean up
1. Navigate to Hosted Zones on the left side of Route 53 console. Click the name of *s3.us-east-1.amazonaws.com* zone. Click Delete and confirm deletion by typing delete. 

![hosted zone](/images/5-Workshop/5.6-Cleanup/delete-zone.png)

2. Disassociate the Route 53 Resolver Rule - myS3Rule from "VPC Onprem" and Delete it. 

![hosted zone](/images/5-Workshop/5.6-Cleanup/vpc.png)

4. Open the CloudFormation console  and delete the two CloudFormation Stacks that you created for this lab:
+ PLOnpremSetup
+ PLCloudSetup

![delete stack](/images/5-Workshop/5.6-Cleanup/delete-stack.png)

5. Delete S3 buckets
+ Open S3 console
+ Choose the bucket we created for the lab, click and confirm empty. Click delete and confirm delete.

![delete s3](/images/5-Workshop/5.6-Cleanup/delete-s3.png)