---
title : "Create a gateway endpoint"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Open the [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:)
2. In the navigation pane, choose **Endpoints**, then click **Create Endpoint**:

{{% notice note %}}
You will see **6 existing VPC endpoints** that support **AWS Systems Manager (SSM)**. These endpoints were deployed automatically by the **CloudFormation Templates** for this workshop.
{{% /notice %}}

![endpoint](/images/s3-vpc/endpoints.png?featherlight=false&width=90pc)

3. In the Create endpoint console:
+ Specify name of the endpoint: ```s3-gwe```
+ In service category, choose **AWS services**

![endpoint](/images/s3-vpc/create-s3-gwe1.png?featherlight=false&width=90pc)

+ In **Services**, type ```s3``` in the search box and choose the service with type **gateway**

![endpoint](/images/s3-vpc/services.png?featherlight=false&width=90pc)

+ For VPC, select **VPC Cloud** from the drop-down.
+ For **Configure route tables**, select the route table that is already associated with **two subnets** (note: this is not the main route table for the VPC, but a second route table created by CloudFormation).

![endpoint](/images/s3-vpc/vpc.png?featherlight=false&width=90pc)

+ **For Policy**, leave the default option, **Full Access**, to allow full access to the service. You will deploy **a VPC endpoint policy** in a later lab module to demonstrate restricting access to **S3 buckets** based on policies.

![endpoint](/images/s3-vpc/policy.png?featherlight=false&width=90pc)

+ Do not add a tag to the VPC endpoint at this time.
+ Click **Create endpoint**, then click x after receiving a successful creation message.

![endpoint](/images/s3-vpc/complete.png?featherlight=false&width=90pc)





