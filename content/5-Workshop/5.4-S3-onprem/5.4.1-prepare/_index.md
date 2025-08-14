---
title : "Prepare the environment"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

To prepare for this part of the workshop you will need to:
+ Deploying a CloudFormation stack 
+ Modifying a VPC route table. 

These components work together to simulate on-premises DNS forwarding and name resolution.

#### Deploy the CloudFormation stack

The CloudFormation template will create additional services to support an on-premises simulation:
+ One Route 53 Private Hosted Zone that hosts Alias records for the PrivateLink S3 endpoint
+ One Route 53 Inbound Resolver endpoint that enables "VPC Cloud" to resolve inbound DNS resolution requests to the Private Hosted Zone
+ One Route 53 Outbound Resolver endpoint that enables "VPC On-prem" to forward DNS requests for S3 to "VPC Cloud"

![route 53 diagram](/images/5-Workshop/5.4-S3-onprem/route53.png)

1. Click the following link to open the [AWS CloudFormation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://s3.amazonaws.com/reinvent-endpoints-builders-session/R53CF.yaml&stackName=PLOnpremSetup). The required template will be pre-loaded into the menu. Accept all default and click Create stack.

![Create stack](/images/5-Workshop/5.4-S3-onprem/create-stack.png)

![Button](/images/5-Workshop/5.4-S3-onprem/create-stack-button.png)

It may take a few minutes for stack deployment to complete. You can continue with the next step without waiting for the deployemnt to finish.

#### Update on-premise private route table

This workshop uses a strongSwan VPN running on an EC2 instance to simulate connectivty between an on-premises datacenter and the AWS cloud. Most of the required components are provisioned before your start. To finalize the VPN configuration, you will modify the "VPC On-prem" routing table to direct traffic destined for the cloud to the strongSwan VPN instance.

1. Open the Amazon EC2 console 

2. Select the instance named infra-vpngw-test. From the Details tab, copy the Instance ID and paste this into your text editor

![ec2 id](/images/5-Workshop/5.4-S3-onprem/ec2-onprem-id.png)

3. Navigate to the VPC menu by using the Search box at the top of the browser window.

4. Click on Route Tables, select the RT Private On-prem route table, select the Routes tab, and click Edit Routes.

![rt](/images/5-Workshop/5.4-S3-onprem/rt.png)

5. Click Add route.
+ Destination: your Cloud VPC cidr range
+ Target: ID of your infra-vpngw-test instance (you saved in your editor at step 1)

![add route](/images/5-Workshop/5.4-S3-onprem/add-route.png)

6. Click Save changes



