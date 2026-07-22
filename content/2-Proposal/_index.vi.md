---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GALAXY BRAIN

## 1. Tổng quan dự án (Project Overview)

**Galaxy Brain** là một ứng dụng thi trắc nghiệm trực tuyến được xây dựng nhằm phục vụ đồ án môn học. Hệ thống tập trung vào các chức năng nghiệp vụ cốt lõi, bảo đảm khả năng vận hành ổn định trong phạm vi người dùng giới hạn, chủ yếu bao gồm giảng viên đánh giá và các thành viên tham gia thử nghiệm.

Ứng dụng được thiết kế theo định hướng **tối giản (Minimalist)** và triển khai trên nền tảng AWS theo mô hình **Serverless**. Kiến trúc này giúp giảm thiểu công việc quản trị hạ tầng, đơn giản hóa quá trình vận hành và tối ưu chi phí triển khai.

### Technology Stack

| Phân loại | Công nghệ sử dụng | Mục đích |
| :--- | :--- | :--- |
| **Frontend** | React, Vite, Amazon S3, Amazon CloudFront | Xây dựng giao diện người dùng, lưu trữ nội dung tĩnh và phân phối nội dung với tốc độ cao. |
| **Compute** | AWS Lambda, Amazon API Gateway | Triển khai Backend sử dụng FastAPI mà không cần quản lý máy chủ ảo. |
| **Database** | Amazon DynamoDB | Lưu trữ dữ liệu trên cơ sở dữ liệu NoSQL Serverless có khả năng tự động mở rộng. |
| **Security** | IAM Role, AWS Secrets Manager, OAC | Kiểm soát quyền truy cập theo nguyên tắc đặc quyền tối thiểu, bảo vệ tài nguyên S3 và thông tin xác thực. |
| **CI/CD** | GitHub Actions | Tự động hóa quy trình build, kiểm thử và triển khai ứng dụng. |

---

## 2. Mục tiêu (Objectives)

### Mục tiêu chung (General Objective)

Xây dựng và hoàn thiện một sản phẩm đồ án học thuật theo quy trình đầy đủ, từ phát triển Frontend và Backend đến triển khai ứng dụng trên nền tảng điện toán đám mây AWS. Hệ thống cần bảo đảm hoạt động ổn định, đáp ứng đầy đủ các chức năng chính và có thể trình diễn thuận lợi trước giảng viên hoặc hội đồng đánh giá.

### Mục tiêu cụ thể (Specific Objectives)

- Triển khai thành công ứng dụng trên AWS theo kiến trúc **Serverless**.
- Tận dụng các dịch vụ thuộc Free Tier của AWS, như AWS Lambda và Amazon DynamoDB, nhằm đưa chi phí vận hành về mức thấp nhất có thể.
- Hoàn thiện tài liệu hướng dẫn triển khai theo từng bước, bảo đảm nội dung rõ ràng, mạch lạc và có thể dễ dàng thực hành lại.

---

## 3. Bài toán và đối tượng sử dụng (Problem Statement)

### Problem Statement

Trong quá trình thực hiện các đồ án học thuật, sinh viên thường phải dành nhiều thời gian cho việc thiết lập và cấu hình hạ tầng AWS, chẳng hạn như Amazon VPC, NAT Gateway hoặc Load Balancer. Điều này có thể làm giảm thời gian dành cho việc phát triển chức năng, đồng thời làm tăng nguy cơ xảy ra sự cố trong quá trình trình diễn sản phẩm.

Galaxy Brain được xây dựng nhằm giải quyết vấn đề trên thông qua kiến trúc **Serverless Cloud-Native**. Việc hạn chế sử dụng máy chủ truyền thống giúp nhóm phát triển giảm bớt công việc quản trị hạ tầng, tập trung nhiều hơn vào chức năng nghiệp vụ và bảo đảm hệ thống vận hành đúng theo yêu cầu.

### Target Audience

- Giảng viên và hội đồng đánh giá đồ án.
- Sinh viên và các thành viên tham gia thử nghiệm ứng dụng.

---

## 4. Kiến trúc hệ thống (System Architecture)

Sơ đồ dưới đây mô tả kiến trúc tổng thể và luồng dữ liệu trong hệ thống Serverless của Galaxy Brain.

![System Architecture](/fcaj-workshop-template/2-Proposal/images/architecture.png)

---

## 5. Tiến độ triển khai (Timeline)

| Giai đoạn | Thời gian | Nội dung công việc |
| :---: | :---: | :--- |
| **Phase 1** | Tuần 1 - 3 | Phân tích yêu cầu, thiết kế giao diện và khởi tạo bảng dữ liệu trên Amazon DynamoDB. |
| **Phase 2** | Tuần 4 - 6 | Phát triển Backend bằng FastAPI và Frontend bằng React. |
| **Phase 3** | Tuần 7 - 9 | Triển khai Backend trên AWS Lambda và tích hợp với Amazon API Gateway. |
| **Phase 4** | Tuần 10 - 12 | Triển khai Frontend trên Amazon S3 và Amazon CloudFront, kiểm thử hệ thống và hoàn thiện báo cáo tổng kết. |

---

## 6. Ngân sách dự kiến (Budget)

Bảng dưới đây trình bày chi phí hạ tầng AWS ước tính theo tháng. Phương án triển khai được thiết kế theo hướng phù hợp với sinh viên và ưu tiên sử dụng các dịch vụ thuộc Free Tier.

| Dịch vụ AWS | Loại cấu hình | Chi phí ước tính/tháng |
| :--- | :--- | :---: |
| **AWS Lambda và Amazon API Gateway** | Sử dụng trong giới hạn Free Tier | $0.00 |
| **Amazon DynamoDB** | Sử dụng dung lượng và lưu lượng trong giới hạn Free Tier | $0.00 |
| **Amazon S3 và Amazon CloudFront** | Lưu trữ nội dung tĩnh và sử dụng băng thông ở mức thấp | Khoảng $1.00 |
| **Amazon VPC và hệ thống mạng** | Kiến trúc Serverless không sử dụng NAT Gateway | $0.00 |
| **Tổng chi phí dự kiến** | | **Khoảng $1.00/tháng** |

> Chi phí thực tế có thể thay đổi tùy theo lưu lượng truy cập, dung lượng lưu trữ, khu vực triển khai và chính sách giá của AWS tại từng thời điểm.

---

## 7. Đánh giá rủi ro (Risk Assessment)

| Rủi ro (Risk) | Mức độ | Kế hoạch giảm thiểu (Mitigation Plan) |
| :--- | :---: | :--- |
| **Độ trễ do Cold Start** | Trung bình | FastAPI khi chạy trên AWS Lambda có thể phản hồi chậm hơn ở lần gọi đầu tiên. Có thể giảm độ trễ bằng cách tối ưu mã nguồn, giảm kích thước gói triển khai và lựa chọn dung lượng bộ nhớ phù hợp (1024 MB). |
| **Lưu lượng truy cập tăng cao** | Thấp | AWS Lambda và Amazon DynamoDB có khả năng tự động mở rộng theo nhu cầu. Tuy nhiên, cần thiết lập giới hạn, giám sát và kiểm thử tải để bảo đảm hệ thống hoạt động ổn định. |
| **Phát sinh chi phí ngoài dự kiến** | Thấp | Thiết lập AWS Budgets và cấu hình cảnh báo qua email khi chi phí đạt hoặc vượt ngưỡng $5. Đồng thời theo dõi số lượng request và mức sử dụng tài nguyên định kỳ. |