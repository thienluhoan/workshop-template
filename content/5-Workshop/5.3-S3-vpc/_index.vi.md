---
title : "Truy cập S3 từ VPC"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Sử dụng Gateway endpoint

Trong phần này, bạn sẽ tạo một Gateway endpoint để truy cập Amazon S3 từ một EC2 instance. Gateway endpoint sẽ cho phép tải một object lên S3 bucket mà không cần sử dụng Internet Công cộng. Để tạo endpoint, bạn phải chỉ định VPC mà bạn muốn tạo endpoint và dịch vụ (trong trường hợp này là S3) mà bạn muốn thiết lập kết nối.

![overview](/images/5-Workshop/5.3-S3-vpc/diagram2.png)

#### Nội dung

- [Tạo gateway endpoint](3.1-create-gwe/)
- [Test gateway endpoint](3.2-test-gwe/)