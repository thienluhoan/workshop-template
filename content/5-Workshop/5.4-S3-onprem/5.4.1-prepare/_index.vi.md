---
title : "Chuẩn bị tài nguyên"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Để chuẩn bị cho phần này của workshop, bạn sẽ cần phải:
+ Triển khai CloudFormation stack
+ Sửa đổi bảng định tuyến VPC.

Các thành phần này hoạt động cùng nhau để mô phỏng DNS forwarding và name resolution.

#### Triển khai CloudFormation stack

Mẫu CloudFormation sẽ tạo các dịch vụ bổ sung để hỗ trợ mô phỏng môi trường truyền thống:
+ Một Route 53 Private Hosted Zone lưu trữ các bản ghi Bí danh (Alias records) cho điểm cuối PrivateLink S3
+ Một Route 53 Inbound Resolver endpoint cho phép "VPC Cloud" giải quyết các yêu cầu resolve DNS gửi đến Private Hosted Zone
+ Một Route 53 Outbound Resolver endpoint cho phép "VPC On-prem" chuyển tiếp các yêu cầu DNS cho S3 sang "VPC Cloud"

![route 53 diagram](/images/5-Workshop/5.4-S3-onprem/route53.png)

1. Click link sau để mở [AWS CloudFormation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://s3.amazonaws.com/reinvent-endpoints-builders-session/R53CF.yaml&stackName=PLOnpremSetup). Mẫu yêu cầu sẽ được tải sẵn vào menu. Chấp nhận tất cả mặc định và nhấp vào Tạo stack.

![Create stack](/images/5-Workshop/5.4-S3-onprem/create-stack.png)

![Button](/images/5-Workshop/5.4-S3-onprem/create-stack-button.png)

Có thể mất vài phút để triển khai stack hoàn tất. Bạn có thể tiếp tục với bước tiếp theo mà không cần đợi quá trình triển khai kết thúc.

####  Cập nhật bảng định tuyến private on-premise 

Workshop này sử dụng StrongSwan VPN chạy trên EC2 instance để mô phỏng khả năng kết nối giữa trung tâm dữ liệu truyền thống và môi trường cloud AWS. Hầu hết các thành phần bắt buộc đều được cung cấp trước khi bạn bắt đầu. Để hoàn tất cấu hình VPN, bạn sẽ sửa đổi bảng định tuyến "VPC on-prem" để hướng lưu lượng đến cloud đi qua StrongSwan VPN instance.

1. Mở Amazon EC2 console 

2. Chọn instance tên infra-vpngw-test. Từ Details tab, copy Instance ID và paste vào text editor của bạn để sử dụng ở những bước tiếp theo

![ec2 id](/images/5-Workshop/5.4-S3-onprem/ec2-onprem-id.png)

3. Đi đến VPC menu bằng cách gõ "VPC" vào Search box

4. Click vào Route Tables, chọn RT Private On-prem route table, chọn Routes tab, và click Edit Routes.

![rt](/images/5-Workshop/5.4-S3-onprem/rt.png)

5. Click Add route.
+ Destination: CIDR block của Cloud VPC
+ Target: ID của infra-vpngw-test instance (bạn đã lưu lại ở bước trên)

![add route](/images/5-Workshop/5.4-S3-onprem/add-route.png)

6. Click Save changes




