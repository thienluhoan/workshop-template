---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Bắt đầu với healthcare data lakes: Sử dụng microservices

Các data lake có thể giúp các bệnh viện và cơ sở y tế chuyển dữ liệu thành những thông tin chi tiết về doanh nghiệp và duy trì hoạt động kinh doanh liên tục, đồng thời bảo vệ quyền riêng tư của bệnh nhân. **Data lake** là một kho lưu trữ tập trung, được quản lý và bảo mật để lưu trữ tất cả dữ liệu của bạn, cả ở dạng ban đầu và đã xử lý để phân tích. data lake cho phép bạn chia nhỏ các kho chứa dữ liệu và kết hợp các loại phân tích khác nhau để có được thông tin chi tiết và đưa ra các quyết định kinh doanh tốt hơn.

Bài đăng trên blog này là một phần của loạt bài lớn hơn về việc bắt đầu cài đặt data lake dành cho lĩnh vực y tế. Trong bài đăng blog cuối cùng của tôi trong loạt bài, *“Bắt đầu với data lake dành cho lĩnh vực y tế: Đào sâu vào Amazon Cognito”*, tôi tập trung vào các chi tiết cụ thể của việc sử dụng Amazon Cognito và Attribute Based Access Control (ABAC) để xác thực và ủy quyền người dùng trong giải pháp data lake y tế. Trong blog này, tôi trình bày chi tiết cách giải pháp đã phát triển ở cấp độ cơ bản, bao gồm các quyết định thiết kế mà tôi đã đưa ra và các tính năng bổ sung được sử dụng. Bạn có thể truy cập các code samples cho giải pháp tại Git repo này để tham khảo.

---

## Hướng dẫn kiến trúc

Thay đổi chính kể từ lần trình bày cuối cùng của kiến trúc tổng thể là việc tách dịch vụ đơn lẻ thành một tập hợp các dịch vụ nhỏ để cải thiện khả năng bảo trì và tính linh hoạt. Việc tích hợp một lượng lớn dữ liệu y tế khác nhau thường yêu cầu các trình kết nối chuyên biệt cho từng định dạng; bằng cách giữ chúng được đóng gói riêng biệt với microservices, chúng ta có thể thêm, xóa và sửa đổi từng trình kết nối mà không ảnh hưởng đến những kết nối khác. Các microservices được kết nối rời thông qua tin nhắn publish/subscribe tập trung trong cái mà tôi gọi là “pub/sub hub”.

Giải pháp này đại diện cho những gì tôi sẽ coi là một lần lặp nước rút hợp lý khác từ last post của tôi. Phạm vi vẫn được giới hạn trong việc nhập và phân tích cú pháp đơn giản của các **HL7v2 messages** được định dạng theo **Quy tắc mã hóa 7 (ER7)** thông qua giao diện REST.

**Kiến trúc giải pháp bây giờ như sau:**

> *Hình 1. Kiến trúc tổng thể; những ô màu thể hiện những dịch vụ riêng biệt.*

---

Mặc dù thuật ngữ *microservices* có một số sự mơ hồ cố hữu, một số đặc điểm là chung:  
- Chúng nhỏ, tự chủ, kết hợp rời rạc  
- Có thể tái sử dụng, giao tiếp thông qua giao diện được xác định rõ  
- Chuyên biệt để giải quyết một việc  
- Thường được triển khai trong **event-driven architecture**

Khi xác định vị trí tạo ranh giới giữa các microservices, cần cân nhắc:  
- **Nội tại**: công nghệ được sử dụng, hiệu suất, độ tin cậy, khả năng mở rộng  
- **Bên ngoài**: chức năng phụ thuộc, tần suất thay đổi, khả năng tái sử dụng  
- **Con người**: quyền sở hữu nhóm, quản lý *cognitive load*

---

## Lựa chọn công nghệ và phạm vi giao tiếp

| Phạm vi giao tiếp                        | Các công nghệ / mô hình cần xem xét                                                        |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ |
| Trong một microservice                   | Amazon Simple Queue Service (Amazon SQS), AWS Step Functions                               |
| Giữa các microservices trong một dịch vụ | AWS CloudFormation cross-stack references, Amazon Simple Notification Service (Amazon SNS) |
| Giữa các dịch vụ                         | Amazon EventBridge, AWS Cloud Map, Amazon API Gateway                                      |

---

## The pub/sub hub

Việc sử dụng kiến trúc **hub-and-spoke** (hay message broker) hoạt động tốt với một số lượng nhỏ các microservices liên quan chặt chẽ.  
- Mỗi microservice chỉ phụ thuộc vào *hub*  
- Kết nối giữa các microservice chỉ giới hạn ở nội dung của message được xuất  
- Giảm số lượng synchronous calls vì pub/sub là *push* không đồng bộ một chiều

Nhược điểm: cần **phối hợp và giám sát** để tránh microservice xử lý nhầm message.

---

## Core microservice

Cung cấp dữ liệu nền tảng và lớp truyền thông, gồm:  
- **Amazon S3** bucket cho dữ liệu  
- **Amazon DynamoDB** cho danh mục dữ liệu  
- **AWS Lambda** để ghi message vào data lake và danh mục  
- **Amazon SNS** topic làm *hub*  
- **Amazon S3** bucket cho artifacts như mã Lambda

> Chỉ cho phép truy cập ghi gián tiếp vào data lake qua hàm Lambda → đảm bảo nhất quán.

---

## Front door microservice

- Cung cấp API Gateway để tương tác REST bên ngoài  
- Xác thực & ủy quyền dựa trên **OIDC** thông qua **Amazon Cognito**  
- Cơ chế *deduplication* tự quản lý bằng DynamoDB thay vì SNS FIFO vì:
  1. SNS deduplication TTL chỉ 5 phút
  2. SNS FIFO yêu cầu SQS FIFO
  3. Chủ động báo cho sender biết message là bản sao

---

## Staging ER7 microservice

- Lambda “trigger” đăng ký với pub/sub hub, lọc message theo attribute  
- Step Functions Express Workflow để chuyển ER7 → JSON  
- Hai Lambda:
  1. Sửa format ER7 (newline, carriage return)
  2. Parsing logic  
- Kết quả hoặc lỗi được đẩy lại vào pub/sub hub

---

## Tính năng mới trong giải pháp

### 1. AWS CloudFormation cross-stack references
Ví dụ *outputs* trong core microservice:
```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
