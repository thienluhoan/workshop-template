---
title : "Access S3 from on-premises"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

+ In this section, you will create an Interface endpoint to access Amazon S3 from a simulated on-premises environment. The Interface endpoint will allow you to route to Amazon S3 over a VPN connection from your simulated on-premises environment.

+ Why using **Interface endpoint**: 
    + Gateway endpoints only work with resources running in the VPC where they are created. Interface endpoints work with resources running in VPC, and also resources running in on-premises environments. Connectivty from your on-premises environment to the cloud can be provided by AWS Site-to-Site VPN or AWS Direct Connect.
    + Interface endpoints allow you to connect to services powered by AWS PrivateLink. These services include some AWS services, services hosted by other AWS customers and partners in their own VPCs (referred to as PrivateLink Endpoint Services), and supported AWS Marketplace Partner services. For this workshop, we will focus on connecting to Amazon S3.

![Interface endpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)



