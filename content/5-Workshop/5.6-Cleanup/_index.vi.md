---
title : "Dọn dẹp tài nguyên"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Dọn dẹp tài nguyên

Xin chúc mừng bạn đã hoàn thành xong lab này!
Trong lab này, bạn đã học về các mô hình kiến trúc để truy cập Amazon S3 mà không sử dụng Public Internet.

+ Bằng cách tạo Gateway endpoint, bạn đã cho phép giao tiếp trực tiếp giữa các tài nguyên EC2 và Amazon S3, mà không đi qua Internet Gateway.
Bằng cách tạo Interface endpoint, bạn đã mở rộng kết nối S3 đến các tài nguyên chạy trên trung tâm dữ liệu trên chỗ của bạn thông qua AWS Site-to-Site VPN hoặc Direct Connect.

#### Dọn dẹp
1. Điều hướng đến Hosted Zones trên phía trái của bảng điều khiển Route 53. Nhấp vào tên của  s3.us-east-1.amazonaws.com zone. Nhấp vào Delete và xác nhận việc xóa bằng cách nhập từ khóa "delete".

![hosted zone](/images/5-Workshop/5.6-Cleanup/delete-zone.png)

2. Disassociate Route 53 Resolver Rule - myS3Rule from "VPC Onprem" and Delete it. 

![hosted zone](/images/5-Workshop/5.6-Cleanup/vpc.png)

4.Mở console của CloudFormation và xóa hai stack CloudFormation mà bạn đã tạo cho bài thực hành này:
+ PLOnpremSetup
+ PLCloudSetup

![delete stack](/images/5-Workshop/5.6-Cleanup/delete-stack.png)

5. Xóa các S3 bucket

+ Mở bảng điều khiển S3
+ Chọn bucket chúng ta đã tạo cho lab, nhấp chuột và xác nhận là empty. Nhấp Delete và xác nhận delete.
+ 
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-s3.png)