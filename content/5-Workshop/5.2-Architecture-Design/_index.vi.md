---
title : "Thiết kế Kiến trúc"
date : 2024-01-01 
weight : 2
chapter : false
alwaysopen: true
pre : " <b> 5.2. </b> "
---

Trong bài thực hành này, chúng ta sẽ đi sâu vào việc phân tích kiến trúc Serverless của hệ thống Galaxy Brain.

Hệ thống được thiết kế theo tư duy Cloud-Native tối giản, loại bỏ hoàn toàn việc quản lý hạ tầng mạng (VPC, Subnet) hay máy chủ truyền thống để tập trung tối đa vào hiệu năng, bảo mật và khả năng tự động mở rộng.

![Sơ đồ Kiến trúc AWS](/fcaj-workshop-template/images/2-Proposal/architecture.png)
*Sơ đồ Kiến trúc AWS của hệ thống Galaxy Brain*

--- 

### Các thành phần cốt lõi của hệ thống
Kiến trúc của chúng ta được chia thành các luồng (Flow) xử lý chuyên biệt:

1. **Luồng Phân phối Giao diện (Frontend Delivery Flow):**
   - Sử dụng **Amazon S3** để lưu trữ các file tĩnh của ứng dụng ReactJS (HTML, CSS, JS).
   - Tích hợp với **Amazon CloudFront (CDN)** để phân phối nội dung toàn cầu. CloudFront sử dụng cơ chế bảo mật OAC (Origin Access Control) để chặn truy cập trực tiếp vào S3.

2. **Luồng Xác thực & Giao tiếp API (API & Auth Flow):**
   - **Amazon API Gateway** đóng vai trò cổng tiếp nhận mọi tương tác từ phía Người dùng và Admin.
   - Request được chuyển tiếp đến **AWS Lambda** - nơi chứa toàn bộ logic Backend viết bằng **FastAPI Monolith**.
   - **AWS Secrets Manager** được gọi để giải mã an toàn các khóa (JWT Secret) phục vụ quá trình xác thực Token của người dùng.

3. **Luồng Dữ liệu & Lưu trữ (Data & Storage Flow):**
   - **Amazon DynamoDB** (cơ sở dữ liệu NoSQL) lưu trữ thông tin User, Quiz, Câu hỏi và Kết quả thi một cách nhanh chóng theo mô hình Single-Table.
   - Hình ảnh đính kèm của các câu hỏi (Media) được upload thẳng lên một Bucket **S3** riêng biệt bằng cơ chế Presigned URL.
   - **IAM Role** được cấp phát theo nguyên tắc đặc quyền tối thiểu (Least Privilege), đóng vai trò làm Execution Role để Lambda có quyền truy xuất DB và S3.

4. **Luồng Giám sát (Observability):**
   - Toàn bộ Log hệ thống, log truy cập và lỗi ứng dụng từ Lambda được tự động đẩy về **Amazon CloudWatch Logs** giúp dễ dàng theo dõi và khắc phục sự cố.