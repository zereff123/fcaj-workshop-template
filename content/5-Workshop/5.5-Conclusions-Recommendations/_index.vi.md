---
title: "Kết luận & Khuyến nghị"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

Đồ án đã chứng minh khả năng triển khai ứng dụng lên AWS một cách tối ưu và tiết kiệm chi phí với kiến trúc Serverless. Dưới đây là hướng dẫn dọn dẹp tài nguyên sau khi demo xong.

---

### Dọn dẹp tài nguyên (Clean up)

Đây là **BƯỚC QUAN TRỌNG NHẤT** đối với mọi đồ án sinh viên sử dụng nền tảng Cloud. Sau khi đã báo cáo xong với hội đồng giám khảo và không còn nhu cầu duy trì hệ thống, bạn BẮT BUỘC phải thực hiện dọn dẹp các tài nguyên đã tạo.

Dù hệ thống Serverless của chúng ta chỉ tính phí khi có người dùng, nhưng việc dọn dẹp để đảm bảo tài khoản sạch sẽ là một thói quen tốt. Dưới đây là trình tự xóa an toàn nhất:

#### 1. Xóa API Gateway & AWS Lambda
1. **API Gateway:**
   - Truy cập dịch vụ **API Gateway**, chọn API `QuizAppAPI` của bạn.
   - Bấm nút **Delete** và xác nhận.
2. **AWS Lambda:**
   - Truy cập **Lambda** -> Functions.
   - Tick chọn hàm `QuizAppBackend`, bấm **Actions** -> **Delete**.

#### 2. Xóa các Bảng DynamoDB
1. Truy cập dịch vụ **DynamoDB**, chọn *Tables*.
2. Chọn từng bảng (Users, Quizzes, Questions, Results) và bấm **Delete**.
3. *Lưu ý:* AWS có thể hỏi bạn có muốn tạo bản sao lưu (Backup) trước khi xóa không. Hãy bỏ qua tùy chọn này nếu bạn không cần giữ lại dữ liệu mẫu để tránh phát sinh chi phí lưu trữ.

#### 3. Xóa IAM Role (Tùy chọn)
1. Truy cập **IAM** -> Roles.
2. Tìm Role `QuizAppLambdaRole` mà bạn đã tạo, chọn và bấm **Delete**.

#### 4. Xóa Frontend CDN và Storage
1. **CloudFront:**
   - Truy cập CloudFront, tick chọn Distribution của bạn, bấm **Disable**.
   - CloudFront cần khoảng 3-5 phút để ngừng phân phối. Sau khi trạng thái chuyển sang *Disabled*, nút **Delete** mới hiện sáng lên. Bấm Delete.
2. **Amazon S3:**
   - Truy cập S3. Bạn không thể xóa Bucket nếu bên trong nó vẫn còn file.
   - Tick chọn Bucket của Frontend, bấm nút **Empty**. Nhập chữ *permanently delete* để xác nhận xóa sạch các file HTML, JS.
   - Sau đó, tick chọn Bucket lần nữa và bấm nút **Delete**.

![Billing Dashboard](/fcaj-workshop-template/images/billing.png)


