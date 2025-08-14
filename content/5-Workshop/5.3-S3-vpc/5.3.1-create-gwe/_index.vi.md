---
title : "Tạo một Gateway Endpoint"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:)
2. Trong thanh điều hướng, chọn **Endpoints**, click **Create Endpoint**:

{{% notice note %}}
Bạn sẽ thấy 6 điểm cuối VPC hiện có hỗ trợ AWS Systems Manager (SSM). Các điểm cuối này được Mẫu CloudFormation triển khai tự động cho workshop này.
{{% /notice %}}

![endpoint](/images/5-Workshop/5.3-S3-vpc/endpoints.png)


3. Trong Create endpoint console:
+ Đặt tên cho endpoint: s3-gwe
+ Trong service category, chọn **aws services**

![endpoint](/images/5-Workshop/5.3-S3-vpc/create-s3-gwe1.png)


+ Trong **Services**, gõ "s3" trong hộp tìm kiếm và chọn dịch vụ với loại **gateway**

![endpoint](/images/5-Workshop/5.3-S3-vpc/services.png)

+ Đối với VPC, chọn **VPC Cloud** từ drop-down menu.
+ Đối với Route tables, chọn bảng định tuyến mà đã liên kết với 2 subnets (lưu ý: đây không phải là bảng định tuyến chính cho VPC mà là bảng định tuyến thứ hai do CloudFormation tạo).

![endpoint](/images/5-Workshop/5.3-S3-vpc/vpc.png)

+ Đối với Policy, để tùy chọn mặc định là Full access để cho phép toàn quyền truy cập vào dịch vụ. Bạn sẽ triển khai VPC endpoint policy trong phần sau để chứng minh việc hạn chế quyền truy cập vào S3 bucket dựa trên các policies.

![endpoint](/images/5-Workshop/5.3-S3-vpc/policy.png)

+ Không thêm tag vào VPC endpoint.
+ Click Create endpoint, click x sau khi nhận được thông báo tạo thành công.

![endpoint](/images/5-Workshop/5.3-S3-vpc/complete.png)
