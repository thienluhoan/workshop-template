---
title : "VPC Endpoint Policies"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Khi bạn tạo một Interface Endpoint  hoặc cổng, bạn có thể đính kèm một chính sách điểm cuối để kiểm soát quyền truy cập vào dịch vụ mà bạn đang kết nối. Chính sách VPC Endpoint là chính sách tài nguyên IAM mà bạn đính kèm vào điểm cuối. Nếu bạn không đính kèm chính sách khi tạo điểm cuối, thì AWS sẽ đính kèm chính sách mặc định cho bạn để cho phép toàn quyền truy cập vào dịch vụ thông qua điểm cuối.

Bạn có thể tạo chính sách chỉ hạn chế quyền truy cập vào các S3 bucket cụ thể. Điều này hữu ích nếu bạn chỉ muốn một số Bộ chứa S3 nhất định có thể truy cập được thông qua điểm cuối.

Trong phần này, bạn sẽ tạo chính sách VPC Endpoint hạn chế quyền truy cập vào S3 bucket được chỉ định trong chính sách VPC Endpoint.

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy.png)

#### Kết nối tới EC2 và xác minh kết nối tới S3. 

1. Bắt đầu một phiên AWS Session Manager mới trên máy chủ có tên là Test-Gateway-Endpoint. Từ phiên này, xác minh rằng bạn có thể liệt kê nội dung của bucket mà bạn đã tạo trong Phần 1: Truy cập S3 từ VPC.

```
aws s3 ls s3://<your-bucket-name>
```
![test](/images/5-Workshop/5.5-Policy/test1.png)

Nội dung của bucket bao gồm hai tệp có dung lượng 1GB đã được tải lên trước đó.

2. Tạo một bucket S3 mới; tuân thủ mẫu đặt tên mà bạn đã sử dụng trong Phần 1, nhưng thêm '-2' vào tên. Để các trường khác là mặc định và nhấp vào **Create**.

![create bucket](/images/5-Workshop/5.5-Policy/create-bucket.png)

3. Tạo bucket thành công.

![Success](/images/5-Workshop/5.5-Policy/create-bucket-success.png)

Policy mặc định cho phép truy cập vào tất cả các S3 Buckets thông qua VPC endpoint.

4. Trong giao diện **Edit Policy**, sao chép và dán theo policy sau, thay thế yourbucketname-2 với tên bucket thứ hai của bạn. Policy này sẽ cho phép truy cập đến bucket mới thông qua VPC endpoint, nhưng không cho phép truy cập đến các bucket còn lại. Chọn **Save** để kích hoạt policy.


```
{
  "Id": "Policy1631305502445",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1631305501021",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": [
      				"arn:aws:s3:::yourbucketname-2",
       				"arn:aws:s3:::yourbucketname-2/*"
       ],
      "Principal": "*"
    }
  ]
}
```

![custom policy](/images/5-Workshop/5.5-Policy/policy2.png)

Cấu hình policy thành công.

![success](/images/5-Workshop/5.5-Policy/success.png)

5. Từ session của bạn trên Test-Gateway-Endpoint instance, kiểm tra truy cập đến S3 bucket bạn tạo ở bước đầu

```
aws s3 ls s3://<yourbucketname>
```

Câu lệnh trả về lỗi bởi vì truy cập vào S3 bucket không có quyền trong VPC endpoint policy.

![error](/images/5-Workshop/5.5-Policy/error.png)

6. Trở lại home directory của bạn trên EC2 instance ```cd~```

+ Tạo file ```fallocate -l 1G test-bucket2.xyz ```
+ Sao chép file lên bucket thứ  2 ```aws s3 cp test-bucket2.xyz s3://<your-2nd-bucket-name>```

![success](/images/5-Workshop/5.5-Policy/test2.png)

Thao tác này được cho phép bởi VPC endpoint policy.

![success](/images/5-Workshop/5.5-Policy/test2-success.png)

Sau đó chúng ta kiểm tra truy cập vào S3 bucket đầu tiên

 ```aws s3 cp test-bucket2.xyz s3://<your-1st-bucket-name>```

 ![fail](/images/5-Workshop/5.5-Policy/test2-fail.png)

 Câu lệnh xảy ra lỗi bởi vì bucket không có quyền truy cập bởi VPC endpoint policy.

Trong phần này, bạn đã tạo chính sách VPC Endpoint cho Amazon S3 và sử dụng AWS CLI để kiểm tra chính sách. Các hoạt động AWS CLI liên quan đến bucket S3 ban đầu của bạn thất bại vì bạn áp dụng một chính sách chỉ cho phép truy cập đến bucket thứ hai mà bạn đã tạo. Các hoạt động AWS CLI nhắm vào bucket thứ hai của bạn thành công vì chính sách cho phép chúng. Những chính sách này có thể hữu ích trong các tình huống khi bạn cần kiểm soát quyền truy cập vào tài nguyên thông qua VPC Endpoint.
