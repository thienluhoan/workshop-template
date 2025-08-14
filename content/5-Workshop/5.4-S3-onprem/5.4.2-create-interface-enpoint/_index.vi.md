---
title : "Tạo một S3 Interface endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Trong phần này, bạn sẽ tạo và kiểm tra Interface Endpoint  S3 bằng cách sử dụng môi trường truyền thống mô phỏng.

1. Quay lại Amazon VPC menu. Trong thanh điều hướng bên trái, chọn Endpoints, sau đó click Create Endpoint.

2. Trong Create endpoint console:
+ Đặt tên interface endpoint
+ Trong Service category, chọn **aws services** 

![name](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint1.png)

3.  Trong Search box, gõ S3 và nhấn Enter. Chọn endpoint có tên com.amazonaws.us-east-1.s3. Đảm bảo rằng cột Type có giá trị Interface.

![service](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint2.png)

4. Đối với VPC, chọn VPC Cloud từ drop-down.
{{% notice warning %}}
Đảm bảo rằng bạn chọn "VPC Cloud" và không phải "VPC On-prem"
{{% /notice %}}
+ Mở rộng **Additional settings** và đảm bảo rằng Enable DNS name *không* được chọn (sẽ sử dụng điều này trong phần tiếp theo của workshop)

![vpc](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint3.png)

5. Chọn 2 subnets trong AZs sau: us-east-1a and us-east-1b

![subnets](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint4.png)

6. Đối với Security group, chọn SGforS3Endpoint:

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint5.png)

7. Giữ default policy - full access và click Create endpoint

![success](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint-success.png)

Chúc mừng bạn đã tạo thành công S3 interface endpoint. Ở bước tiếp theo, chúng ta sẽ kiểm tra interface endpoint.