---
title : "Kiểm tra Interface Endpoint"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Lấy regional DNS name (tên DNS khu vực) của S3 interface endpoint
1. Trong Amazon VPC menu, chọn Endpoints.

2. Click tên của endpoint chúng ta mới tạo ở mục 4.2: s3-interface-endpoint. Click details và lưu lại regional DNS name của endpoint (cái đầu tiên) vào text-editor của bạn để dùng ở các bước sau.

![dns name](/images/5-Workshop/5.4-S3-onprem/dns.png)

#### Kết nối đến EC2 instance ở trong "VPC On-prem" (giả lập môi trường truyền thống)

1. Đi đến **Session manager** bằng cách gõ "session manager" vào ô tìm kiếm

2. Click **Start Session**, chọn EC2 instance có tên **Test-Interface-Endpoint**. EC2 instance này đang chạy trên "VPC On-prem" và sẽ được sử dụng để kiểm tra kết nối đến Amazon S3 thông qua Interface endpoint. Session Manager sẽ mở 1 browser tab mới với shell prompt: **sh-4.2 $**

![Start session](/images/5-Workshop/5.4-S3-onprem/start-session.png)

3. Đi đến ssm-user's home directory với lệnh "cd ~"

4. Tạo 1 file tên testfile2.xyz
```
fallocate -l 1G testfile2.xyz
```

![user](/images/5-Workshop/5.4-S3-onprem/cli1.png)

5. Copy file vào S3 bucket mình tạo ở section 4.2
```
aws s3 cp --endpoint-url https://bucket.<Regional-DNS-Name> testfile2.xyz s3://<your-bucket-name>
``` 
+ Câu lệnh này yêu cầu thông số --endpoint-url, bởi vì bạn cần sử dụng DNS name chỉ định cho endpoint để truy cập vào S3 thông qua Interface endpoint.
+ Không lấy ' * ' khi copy/paste tên DNS khu vực.
+ Cung cấp tên S3 bucket của bạn

![copy file](/images/5-Workshop/5.4-S3-onprem/cli2.png)

Bây giờ tệp đã được thêm vào bộ chứa S3 của bạn. Hãy kiểm tra bộ chứa S3 của bạn trong bước tiếp theo.

#### Kiểm tra Object trong S3 bucket

1. Đi đến S3 console
2. Click Buckets
3. Click tên bucket của bạn và bạn sẽ thấy testfile2.xyz đã được thêm vào s3 bucket của bạn

![check bucket](/images/5-Workshop/5.4-S3-onprem/check-bucket.png)




