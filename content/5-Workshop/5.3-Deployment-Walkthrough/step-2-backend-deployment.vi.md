---
title: "Triển khai Backend"
date: 2026-07-12
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

Sau khi Cơ sở dữ liệu DynamoDB đã sẵn sàng, bước tiếp theo là đưa mã nguồn Backend (FastAPI) lên đám mây. Chúng ta sẽ sử dụng dịch vụ điện toán không máy chủ cực kỳ mạnh mẽ của AWS là **AWS Lambda**, kết hợp với **Amazon API Gateway** để định tuyến request.

Khác với kiến trúc cũ chạy Docker 24/7 gây lãng phí, AWS Lambda chỉ tính tiền khi có người gọi API, giúp bạn tiết kiệm chi phí tối đa.

---

### Khởi tạo IAM Execution Role cho Lambda
Trước khi tạo Lambda, chúng ta cần tạo một chiếc "thẻ căn cước" (IAM Role) để cấp quyền cho Lambda được phép đọc/ghi vào bảng DynamoDB và ghi Log.

1. Truy cập dịch vụ **IAM (Identity and Access Management)**, ở menu trái chọn **Roles** -> *Create role*.
2. Trusted entity type: Chọn **AWS service**.
3. Use case: Chọn **Lambda** rồi nhấn Next.
4. Tại bước Add permissions, tìm và tích chọn 2 policies sau:
   - `AWSLambdaBasicExecutionRole` (Quyền ghi log lên CloudWatch).
   - `AmazonDynamoDBFullAccess` (Quyền thao tác với DynamoDB. *Lưu ý: Trong thực tế dự án lớn bạn nên tạo custom policy chỉ cấp quyền cho đúng cái ARN đã copy ở Bước 1 để bảo mật tuyệt đối*).
5. Nhấn Next, đặt tên Role là `QuizAppLambdaRole`.
6. Nhấn **Create role**.

![IAM Role Permissions](/fcaj-workshop-template/images/iam-role.png)

---

### Triển khai mã nguồn lên AWS Lambda
Do Backend của chúng ta sử dụng Python (FastAPI) và các thư viện ngoài (như Mangum để tương thích Lambda), bạn cần chuẩn bị sẵn một file `.zip` chứa toàn bộ code và dependencies (thư mục `venv` hoặc `site-packages`).

1. Truy cập dịch vụ **AWS Lambda**, chọn **Functions** -> *Create function*.
2. Chọn **Author from scratch**.
3. Basic information:
   - Function name: `QuizAppBackend`
   - Runtime: Chọn **Python 3.13** (Hoặc phiên bản khớp với local của bạn).
   - Architecture: **x86_64**.
4. Permissions:
   - Expand mục *Change default execution role*.
   - Chọn **Use an existing role**.
   - Chọn Role `QuizAppLambdaRole` bạn vừa tạo ở trên.
5. Nhấn **Create function**.
6. Tại trang chi tiết Function, cuộn xuống phần **Code source**, nhấn nút **Upload from** -> chọn **.zip file** và tải file code nén của bạn lên. Lưu lại.
7. Cuộn xuống phần **Runtime settings**, nhấn *Edit* và đổi trường **Handler** thành `app.handler` (Vì file code chính của bạn tên là `app.py` và đối tượng Mangum tên là `handler`).

![Lambda Code Source](/fcaj-workshop-template/images/lambda-code.png)

---

### Định tuyến bằng Amazon API Gateway
AWS Lambda không tự nhiên mở cửa ra Internet. Chúng ta cần một dịch vụ làm cổng bảo vệ, đó là API Gateway.

1. Chuyển sang dịch vụ **API Gateway**, cuộn tìm **HTTP API** và nhấn *Build*.
2. Ở phần Integrations, nhấn *Add integration*:
   - Chọn **Lambda**.
   - AWS Region: Cùng region với Lambda.
   - Lambda function: Chọn `QuizAppBackend`.
3. Đặt tên API name là `QuizAppAPI`. Nhấn Next.
4. Configure routes:
   - Method: Chọn **ANY**.
   - Resource path: Nhập `/{proxy+}` (Cấu hình này bắt mọi request và đẩy toàn bộ cho FastAPI tự phân luồng).
   - Integration target: Chọn Lambda function của bạn.
5. Nhấn Next, để mặc định stage `$default`, và nhấn **Create**.
6. Khi API Gateway được tạo thành công, bạn sẽ nhận được một chuỗi **Invoke URL** (Ví dụ: `https://abcd12345.execute-api.ap-southeast-1.amazonaws.com`). Để lấy được link này, bạn hãy nhìn sang menu bên trái, bấm vào mục **Stages** nhé.
7. Hãy **Copy URL** này, đây chính là "cửa ngõ" để Frontend gọi vào Backend của bạn!

![API Gateway Routes](/fcaj-workshop-template/images/api-gateway.png)
