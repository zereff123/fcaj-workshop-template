---
title: "Khởi tạo Database"
date: 2026-07-12
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

Trái tim của hệ thống Galaxy Brain là Cơ sở dữ liệu lưu trữ thông tin câu hỏi, đề thi và điểm số của người dùng. Để đáp ứng tiêu chí Serverless và tối ưu ngân sách, chúng ta sẽ sử dụng **Amazon DynamoDB** thay vì các cơ sở dữ liệu quan hệ truyền thống.

---

### Khởi tạo các bảng Amazon DynamoDB
Dựa trên thiết kế kiến trúc đa bảng (Multi-Table) của dự án, chúng ta sẽ cần khởi tạo 4 bảng riêng biệt. Bạn cần lặp lại các bước dưới đây 4 lần để tạo đủ 4 bảng.

1. Đăng nhập vào AWS Management Console, tìm kiếm dịch vụ **DynamoDB** và chọn *Create table*.
2. Cài đặt thông tin bảng (Table details). Lần lượt tạo 4 bảng với cấu hình khóa (Key) như sau:
   - **Bảng 1 (Người dùng):**
     - Table name: `QuizApp-Users-dev`
     - Partition key: `user_id` (String)
     - Sort key: *(Bỏ trống)*
   - **Bảng 2 (Danh sách đề thi):**
     - Table name: `QuizApp-Quizzes-dev`
     - Partition key: `quiz_id` (String)
     - Sort key: *(Bỏ trống)*
   - **Bảng 3 (Ngân hàng câu hỏi):**
     - Table name: `QuizApp-Questions-dev`
     - Partition key: `quiz_id` (String)
     - Sort key: `question_id` (String)
   - **Bảng 4 (Kết quả thi):**
     - Table name: `QuizApp-Results-dev`
     - Partition key: `user_id` (String)
     - Sort key: `result_id` (String)

3. Cài đặt dung lượng (Table settings) - Áp dụng chung cho cả 4 bảng:
   - Chọn **Customize settings**.
   - Capacity calculator: Chọn **Provisioned** (Đây là mức cấu hình được hưởng Free Tier).
   - Read capacity và Write capacity: Đặt cả hai ở mức **5** để đảm bảo luôn nằm trong giới hạn miễn phí 25GB của AWS.
4. Cuộn xuống dưới cùng và nhấn **Create table**. 
5. Quá trình tạo bảng DynamoDB diễn ra cực kỳ nhanh chóng. Sau khi tạo xong cả 4 bảng, hãy vào lại menu **Tables** để kiểm tra trạng thái *Active*.


![DynamoDB Tables](/fcaj-workshop-template/images/dynamodb-tables.png)

---

### Lấy thông tin ARN (Amazon Resource Name)
Khi các bảng đã được tạo thành công, chúng ta cần lưu lại địa chỉ định danh (ARN) để cấp quyền cho Backend (AWS Lambda) có thể đọc ghi dữ liệu. Ở bước này, bạn chỉ cần lấy ARN của 1 bảng đại diện để biết định dạng.

1. Bấm vào tên bảng `QuizApp-Users-dev`.
2. Ở tab **Overview**, cuộn xuống phần *General information*.
3. Tìm dòng **Amazon Resource Name (ARN)** (Có dạng `arn:aws:dynamodb:ap-southeast-1:123456789:table/QuizApp-Users-dev`).
4. Hãy **Copy** phần tiền tố ARN này để cấu hình ở Bước 2.

---

### Nạp dữ liệu mẫu (Seed Data)
Các bảng DynamoDB của bạn hiện đang trống không. Để hệ thống Frontend có dữ liệu hiển thị (các câu hỏi trắc nghiệm, thông tin người dùng) ngay khi kết nối, chúng ta cần chạy một script Python để tự động bơm (seed) dữ liệu mẫu vào 4 bảng này.

1. Đảm bảo bạn đã cài đặt **Python** và thư viện **Boto3** (`pip install boto3`) trên máy tính.
2. Đảm bảo bạn đã cấu hình thông tin xác thực AWS CLI trên máy (chạy lệnh `aws configure` và nhập Access Key của IAM User).
3. Mở Terminal tại thư mục gốc của dự án mã nguồn `e:\quiz_aws`.
4. Chạy câu lệnh sau để tiến hành nạp dữ liệu:
   ```bash
   python infrastructure/seed_dynamodb.py --env dev --region ap-southeast-1
   ```
5. Đợi Terminal báo thành công `=== Seed hoan thanh! ===`. Lúc này, bạn có thể quay lại giao diện DynamoDB, bấm vào mục **Explore items**, chọn các bảng vừa tạo để tận mắt thấy dữ liệu đã được thêm vào một cách kỳ diệu!
