---
title: "Blog 1"
date: 2026-07-20
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# SESSION POLICIES TRONG AMAZON EKS POD IDENTITY

Database là nơi lưu trữ và quản lý dữ liệu của ứng dụng. Trong thực tế, cơ sở dữ liệu thường được chia thành hai nhóm chính là SQL và NoSQL. SQL phù hợp với dữ liệu có cấu trúc rõ ràng và nhiều mối quan hệ, trong khi NoSQL linh hoạt hơn, dễ mở rộng và phù hợp với các ứng dụng có lượng truy cập lớn.

Các điểm chính cần nắm:

* Relational Database (SQL) lưu dữ liệu theo bảng, gồm hàng và cột.
* SQL phù hợp với hệ thống cần tính nhất quán cao và các giao dịch phức tạp.
* NoSQL không yêu cầu cấu trúc dữ liệu cố định, có thể lưu theo dạng key-value hoặc document.
* NoSQL phù hợp với ứng dụng web, mobile, game, IoT và các hệ thống có lưu lượng truy cập lớn.
* Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được AWS quản lý hoàn toàn.
* DynamoDB có tốc độ đọc và ghi nhanh, độ trễ thấp và khả năng mở rộng tự động.
* Dữ liệu được sao chép trên nhiều Availability Zone để tăng tính sẵn sàng và độ bền.
* DynamoDB hỗ trợ hai chế độ tính phí là On-Demand và Provisioned Capacity.
* Có thể tích hợp với AWS Lambda, API Gateway, IAM và Amazon CloudWatch.

Amazon DynamoDB đặc biệt phù hợp với các ứng dụng cần tốc độ xử lý nhanh, khả năng mở rộng linh hoạt và không muốn tự quản lý máy chủ cơ sở dữ liệu, chẳng hạn như ứng dụng web, game online, hệ thống IoT, quản lý phiên đăng nhập và ứng dụng thời gian thực.

![Blog picture](/fcaj-workshop-template/images/blog.png)

...Link...

...Hướng dẫn...