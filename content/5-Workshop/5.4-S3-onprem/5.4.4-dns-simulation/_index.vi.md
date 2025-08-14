---
title : "Mô phỏng On-premises DNS "
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

 AWS PrivateLink endpoint có một địa chỉ IP cố định trong từng AZ nơi chúng được triển khai, trong suốt thời gian tồn tại của endpoint (cho đến khi endpoint bị xóa). Các địa chỉ IP này được gắn vào Elastic network interface (ENI). AWS khuyến nghị sử dụng DNS để resolve địa chỉ IP cho endpoint để các ứng dụng downstream sử dụng địa chỉ IP mới nhất khi ENIs được thêm vào AZ mới hoặc bị xóa theo thời gian.

Trong phần này, bạn sẽ tạo một quy tắc chuyển tiếp (forwarding rule) để gửi các yêu cầu resolve DNS từ môi trường truyền thống (mô phỏng) đến Private Hosted Zone trên Route 53. Phần này tận dụng cơ sở hạ tầng do CloudFormation triển khai trong phần Chuẩn bị môi trường.

#### Tạo DNS Alias Records cho Interface endpoint
1. Click link để đi đến [Route 53 management console](https://us-east-1.console.aws.amazon.com/route53/v2/hostedzones?region=us-east-1#) (Hosted Zones section).  Mẫu CloudFormation mà bạn triển khai trong phần Chuẩn bị môi trường đã tạo Private Hosted Zone này. Nhấp vào tên của Private Hosted Zone, s3.us-east-1.amazonaws.com:

![hosted zone](/images/5-Workshop/5.4-S3-onprem/hosted-zone.png)

2. Tạo 1 record mới trong Private Hosted Zone:

![Create record](/images/5-Workshop/5.4-S3-onprem/create-record1.png)

+ Giữ nguyên Record name và record type
+ Alias Button: click để enable
+ Route traffic to: Alias to VPC Endpoint
+ Region: US East (N. Virginia) [us-east-1]
+ Chọn endpoint: Paste tên (Regional VPC Endpoint DNS) bạn đã lưu lại ở phần 4.3

![record1](/images/5-Workshop/5.4-S3-onprem/record1.png)

3. Click Add another record, và add 1 cái record thứ 2 sử dụng những thông số sau:
+ Record name: *.
+ Record type: giữ giá trị default (type A)
+ Alias Button: Click để enable
+ Route traffic to: Alias to VPC Endpoint
+ Region: US East (N. Virginia) [us-east-1]
+ Chọn endpoint: Paste tên (Regional VPC Endpoint DNS) bạn đã lưu lại ở phần 4.3
+ Click **Create records** 

![record 2](/images/5-Workshop/5.4-S3-onprem/record2.png)

Record mới xuất hiện trên giao diện Route 53.

![result](/images/5-Workshop/5.4-S3-onprem/result.png)

#### Tạo một Resolver Forwarding Rule

**Route 53 Resolver Forwarding Rules** cho phép bạn chuyển tiếp các DNS queries từ VPC của bạn đến các nguồn khác để resolve name. Bên ngoài môi trường workshop, bạn có thể sử dụng tính năng này để chuyển tiếp các DNS queries từ VPC của bạn đến các máy chủ DNS chạy trên on-premises. Trong phần này, bạn sẽ mô phỏng một on-premises conditional forwarder bằng cách tạo một forwarding rule để chuyển tiếp các DNS queries for Amazon S3 đến một Private Hosted Zone chạy trong "VPC Cloud" để resolve PrivateLink interface endpoint regional DNS name.

1. Từ giao diện  **Route 53**, chọn **Inbound endpoints** trên thanh bên trái

2. Trong giao diện **Inbound endpoint**, Chọn ID của Inbound endpoint.

![Inbound endpoint](/images/5-Workshop/5.4-S3-onprem/route53-1.png)

3. Sao chép 2 địa chỉ IP trong danh sách vào trình chỉnh sửa.

![Ip addresses](/images/5-Workshop/5.4-S3-onprem/route53-2.png)

4. Từ giao diện Route 53, chọn  **Resolver** > **Rules** và chọn **Create rule**

![Ip addresses](/images/5-Workshop/5.4-S3-onprem/route53-3.png)

5. Trong giao diện **Create rule**

+ Name: myS3Rule
+ Rule type: Forward
+ Domain name: s3.us-east-1.amazonaws.com

![create rule](/images/5-Workshop/5.4-S3-onprem/route53-4.png)

+ VPC: VPC On-prem
+ Outbound endpoint: VPCOnpremOutboundEndpoint

![create rule](/images/5-Workshop/5.4-S3-onprem/route53-5.png)

+ Target IP Addresses: điền cả hai IP bạn đã lưu trữ trên trình soạn thảo (inbound endpoint addresses) và sau đó chọn **Submit**

![create rule](/images/5-Workshop/5.4-S3-onprem/route53-6.png)

Bạn đã tạo thành công resolver forwarding rule. 

![create rule](/images/5-Workshop/5.4-S3-onprem/route53-7.png)

#### Kiểm tra on-premises DNS mô phỏng.

1. Kết nối đến **Test-Interface-Endpoint EC2 instance** với **Session Manager**

![create rule](/images/5-Workshop/5.4-S3-onprem/test1.png)

2. Kiểm tra DNS resolution. Lệnh dig sẽ trả về địa chỉ IP được gán cho VPC endpoint interface đang chạy trên VPC (địa chỉ IP của bạn sẽ khác):

```
dig +short s3.us-east-1.amazonaws.com 
```
{{% notice note %}}
Các địa chỉ IP được trả về là các địa chỉ IP VPC enpoint, KHÔNG phải là các địa chỉ IP Resolver mà bạn đã dán từ trình chỉnh sửa văn bản của mình. Các địa chỉ IP của  Resolver endpoint  và  VPC endpoin trông giống nhau vì chúng đều từ khối CIDR VPC Cloud.
{{% /notice %}}

![create rule](/images/5-Workshop/5.4-S3-onprem/dig.png)

3. Truy cập vào menu VPC (phần Endpoints), chọn S3 interface endpoint. Nhấp vào tab Subnets và xác nhận rằng các địa chỉ IP được trả về bởi lệnh Dig khớp với VPC endpoint:

![create rule](/images/5-Workshop/5.4-S3-onprem/subnet.png)

4. Hãy quay lại shell của bạn và sử dụng AWS CLI để kiểm tra danh sách các bucket S3 của bạn:

```
aws s3 ls --endpoint-url https://s3.us-east-1.amazonaws.com
```

![create rule](/images/5-Workshop/5.4-S3-onprem/endpoint.png)

5. Kết thúc phiên làm việc của Session Manager của bạn:

![create rule](/images/5-Workshop/5.4-S3-onprem/terminal.png)


Trong phần này, bạn đã tạo một  **Interface Endpoint**  cho Amazon S3. Điểm cuối này có thể được truy cập từ on-premises thông qua Site-to-Site VPN hoặc AWS Direct Connect. Các điểm cuối Route 53 Resolver outbound giả lập chuyển tiếp các yêu cầu DNS từ on-premises đến một Private Hosted Zone đang chạy trên đám mây. Các điểm cuối Route 53 inbound nhận yêu cầu giải quyết và trả về một phản hồi chứa địa chỉ IP của  **Interface Endpoint**  VPC. Sử dụng DNS để giải quyết các địa chỉ IP của điểm cuối cung cấp tính sẵn sàng cao trong trường hợp một Availability Zone gặp sự cố.