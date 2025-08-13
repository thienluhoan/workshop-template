---
title : "Kiểm tra Gateway Endpoint"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Tạo S3 bucket

1. Đi đến S3 management console
2. Trong Bucket console, chọn **Create bucket**

![Create bucket](/images/s3-vpc/create-bucket.png?featherlight=false&width=90pc)

3. Trong Create bucket console
+ Đặt tên bucket: chọn 1 tên mà không bị trùng trong phạm vi toàn cầu (gợi ý: lab\<số-lab\>\<tên-bạn\>)

![Bucket name](/images/s3-vpc/bucket-name.png?featherlight=false&width=90pc)

+ Giữ nguyên giá trị của các fields khác (default)
+ Kéo chuột xuống và chọn **Create bucket**

![Create](/images/s3-vpc/create-button.png?featherlight=false&width=90pc)    

+ Tạo thành công S3 bucket

![Success](/images/s3-vpc/bucket-success.png?featherlight=false&width=90pc)

#### Kết nối với EC2 bằng session manager

+ Trong workshop này, bạn sẽ dùng AWS Session Manager để kết nối đến các EC2 instances. Session Manager là 1 tính năng trong dịch vụ Systems Manager được quản lý hoàn toàn bởi AWS. System manager cho phép bạn quản lý Amazon EC2 instances và các máy ảo on-premises (VMs)thông qua 1 browser-based shell. Session Manager cung cấp khả năng quản lý phiên bản an toàn và có thể kiểm tra mà không cần mở cổng vào, duy trì máy chủ bastion host hoặc quản lý khóa SSH.

+ First cloud journey [Lab](https://000058.awsstudygroup.com/1-introduce/) để hiểu sâu hơn về Session manager.

1. Trong AWS Management Console, gõ Systems Manager trong ô tìm kiếm và nhấn Enter:

![system manager](/images/s3-vpc/sm.png?featherlight=false&width=90pc)

2. Từ **Systems Manager** menu, tìm **Node Management** ở thanh bên trái và chọn **Session Manager**:

![system manager](/images/s3-vpc/sm1.png?featherlight=false&width=90pc)

3. Click Start Session, và chọn EC2 instance tên **Test-Gateway-Endpoint**. 
{{% notice info %}}
Phiên bản EC2 này đã chạy trong "VPC cloud" và sẽ được dùng để kiểm tra khả năng kết nối với Amazon S3 thông qua điểm cuối Cổng mà bạn vừa tạo (s3-gwe). {{% /notice %}}

![Start session](/images/s3-vpc/start-session.png?featherlight=false&width=90pc)

Session Manager sẽ mở browser tab mới với shell prompt: sh-4.2 $

![Success](/images/s3-vpc/start-session-success.png?featherlight=false&width=90pc)

Bạn đã bắt đầu phiên kết nối đến EC2 trong VPC Cloud thành công. Trong bước tiếp theo, chúng ta sẽ tạo một  S3 bucket và một tệp trong đó.
#### Create a file and upload to s3 bucket

1. Đổi về ssm-user's thư mục bằng lệnh "cd ~" 

![Change user's dir](/images/s3-vpc/cli1.png?featherlight=false&width=90pc)

2. Tạo 1 file để kiểm tra bằng lệnh "fallocate -l 1G testfile.xyz", 1 file tên "testfile.xyz" có kích thước 1GB sẽ được tạo.

![Create file](/images/s3-vpc/cli-file.png?featherlight=false&width=90pc)

3. Tải file mình vừa tạo lên S3 với lệnh "aws s3 cp testfile.xyz s3://your-bucket-name". Thay your-bucket-name bằng tên S3 bạn đã tạo.

![Uploaded](/images/s3-vpc/uploaded.png?featherlight=false&width=90pc)

Bạn đã tải thành công tệp lên bộ chứa S3 của mình. Bây giờ bạn có thể kết thúc session.

#### Kiểm tra object trong S3 bucket

1. Đi đến S3 console.  
2. Click tên s3 bucket của bạn
3. Trong Bucket console, bạn sẽ thấy tệp bạn đã tải lên S3 bucket của mình

![Check S3](/images/s3-vpc/check-s3-bucket.png?featherlight=false&width=90pc)

#### Tóm tắt

Chúc mừng bạn đã hoàn thành truy cập S3 từ VPC. Trong phần này, bạn đã tạo gateway endpoint cho Amazon S3 và sử dụng AWS CLI để tải file lên. Quá trình tải lên hoạt động vì gateway endpoint cho phép giao tiếp với S3 mà không cần Internet gateway gắn vào "VPC Cloud". Điều này thể hiện chức năng của gateway endpoint như một đường dẫn an toàn đến S3 mà không cần đi qua public Internet.