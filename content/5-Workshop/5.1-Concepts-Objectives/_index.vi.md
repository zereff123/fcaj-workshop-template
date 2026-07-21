---
title : "Lý thuyết & Mục tiêu"
date : 2026-07-12 
weight : 1
chapter : false
alwaysopen: true
pre : " <b> 5.1. </b> "
---
Trong phần này, chúng ta sẽ tìm hiểu về các khái niệm cốt lõi tạo nên kiến trúc của hệ thống Galaxy Brain. 

### Mục tiêu đồ án

Đồ án tập trung vào việc thiết kế và xây dựng một hệ thống thi trắc nghiệm trực tuyến (Quiz App) hoàn chỉnh. Đảm bảo luồng dữ liệu thông suốt từ việc lưu trữ cơ sở dữ liệu, xử lý nghiệp vụ Backend, cho đến phân phối giao diện Frontend tới người dùng cuối với tốc độ nhanh chóng và tối ưu chi phí thông qua kiến trúc Cloud-Native Serverless.

### Các khái niệm AWS cơ bản
- **Amazon S3 & CloudFront:** Bộ đôi hoàn hảo để lưu trữ website tĩnh (Frontend) và phân phối nội dung với tốc độ cực nhanh trên toàn cầu, thay thế cho các máy chủ web truyền thống.
- **Amazon API Gateway:** Cổng giao tiếp an toàn, tiếp nhận các HTTP Request từ Frontend và định tuyến xuống Backend.
- **AWS Lambda:** Dịch vụ điện toán không máy chủ (Serverless compute). Ứng dụng Backend (viết bằng FastAPI) sẽ được chạy tự động trên Lambda mà không cần quản trị máy chủ ảo hay vùng mạng (VPC).
- **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL với khả năng tự động mở rộng, cực kỳ phù hợp cho kiến trúc Serverless để lưu trữ dữ liệu người dùng, bài thi và kết quả.


