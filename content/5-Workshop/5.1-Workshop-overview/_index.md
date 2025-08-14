---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### VPC endpoints
+ **VPC endpoints** are virtual devices. They are horizontally scaled, redundant, and highly available VPC components. They allow communication between your compute resources and AWS services without imposing availability risks.
+ Compute resources running in VPC can access  **Amazon S3**  using a Gateway endpoint. PrivateLink interface endpoints can be used by compute resources running in VPC or on-premises.

#### Workshop overview
In this workshop, you will use two VPCs. 
+ **"VPC Cloud"** is for cloud resources such as a  **Gateway endpoint** and an EC2 instance to test with. 
+ **"VPC On-Prem"** simulates an on-premises environment such as a factory or corporate datacenter. An EC2 instance running strongSwan VPN software has been deployed in "VPC On-prem" and automatically configured to establish a Site-to-Site VPN tunnel with AWS Transit Gateway. This VPN simulates connectivity from an on-premises location to the AWS cloud. To minimize costs, only one VPN instance is provisioned to support this workshop. When planning VPN connectivity for your production workloads, AWS recommends using multiple VPN devices for high availability.

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)