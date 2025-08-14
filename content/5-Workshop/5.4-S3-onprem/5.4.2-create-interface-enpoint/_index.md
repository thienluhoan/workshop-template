---
title : "Create an S3 Interface endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

In this section you will create and test an S3 interface endpoint using the simulated on-premises environment deployed as part of this workshop.

1. Return to the Amazon VPC menu. In the navigation pane, choose Endpoints, then click Create Endpoint.

2. In Create endpoint console:
+ Name the interface endpoint
+ In Service category, choose **aws services** 

![name](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint1.png)

3.  In the Search box, type S3 and press Enter. Select the endpoint named com.amazonaws.us-east-1.s3. Ensure that the Type column indicates Interface.

![service](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint2.png)

4. For VPC, select VPC Cloud from the drop-down.
{{% notice warning %}}
Make sure to choose "VPC Cloud" and not "VPC On-prem"
{{% /notice %}}
+ Expand **Additional settings** and ensure that Enable DNS name is *not* selected (we will use this in the next part of the workshop)

![vpc](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint3.png)

5. Select 2 subnets in the following AZs: us-east-1a and us-east-1b

![subnets](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint4.png)

6. For Security group, choose SGforS3Endpoint:

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint5.png)

7. Keep the default policy - full access and click Create endpoint

![success](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint-success.png)

Congratulation on successfully creating S3 interface endpoint. In the next step, we will test the interface endpoint.