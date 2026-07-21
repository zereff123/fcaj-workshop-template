---
title: "Triển khai Frontend"
date: 2026-07-12
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Cách rẻ nhất, nhanh nhất và bảo mật nhất để host một ứng dụng Single Page Application (như ReactJS) là sử dụng bộ đôi Amazon S3 và Amazon CloudFront CDN kết hợp với OAC (Origin Access Control).

Với kiến trúc này, mã nguồn tĩnh (HTML, CSS, JS) sẽ được lưu kín trong S3 và chỉ được phân phối ra ngoài qua CDN (CloudFront) với tốc độ cực nhanh.

---

### Build code ReactJS
Trước khi đưa lên Cloud, Frontend của bạn cần biết địa chỉ để giao tiếp với Backend.
1. Mở mã nguồn ReactJS của bạn. Tìm file chứa biến cấu hình API URL (Thường là `.env` hoặc file config hằng số).
2. Thay thế `http://localhost:8000` bằng URL của **API Gateway** mà bạn lấy được ở Bước trước (Ví dụ: `https://abcd12345.execute-api.ap-southeast-1.amazonaws.com`).
3. Mở Terminal và chạy lệnh `npm run build` (hoặc `yarn build`).
4. Kết quả là bạn sẽ thu được một thư mục có tên là `dist` (hoặc `build`) chứa các file tĩnh đã được nén tối ưu.

---

### Lưu trữ website tĩnh trên Amazon S3 (Kín hoàn toàn)
Chúng ta sẽ lưu file vào S3 nhưng **tuyệt đối KHÔNG mở Public Access** để tránh rò rỉ mã nguồn.

1. Truy cập dịch vụ **S3**, nhấn *Create bucket*. Đặt tên bucket (VD: `quiz-app-frontend-dev-993c6d33`).
2. **Block Public Access settings for this bucket:** HÃY ĐẢM BẢO ô *Block all public access* **ĐƯỢC TÍCH CHỌN**.
3. Nhấn *Create bucket*.
4. Bấm vào Bucket vừa tạo, chuyển sang tab **Objects**, nhấn Upload và tải toàn bộ các file TRONG thư mục `dist` của bạn lên S3.

![S3 Frontend Bucket](/fcaj-workshop-template/images/s3-upload.png)

---

### Phân phối toàn cầu bằng CloudFront & OAC
Đây là bước quan trọng nhất để đưa website ra Internet một cách bảo mật với HTTPS.

1. Truy cập dịch vụ **CloudFront**, nhấn *Create a CloudFront distribution*.
2. **Origin domain:** Chọn S3 bucket của bạn từ danh sách gợi ý.
3. **Origin access:** 
   - Chọn **Origin access control settings (recommended)**.
   - Nhấn *Create control setting*, giữ nguyên tên mặc định và nhấn Create.
   - Hành động này sẽ yêu cầu AWS tự động sinh ra một đoạn Policy để S3 chỉ cho phép CloudFront này truy cập.
4. **Viewer protocol policy:** Chọn *Redirect HTTP to HTTPS*.
5. **Default root object:** Cuộn xuống dưới cùng và gõ `index.html`.
6. Nhấn *Create distribution*.

Ngay sau khi tạo xong, CloudFront sẽ hiện một thanh thông báo màu xanh yêu cầu bạn cập nhật S3 Bucket Policy.
1. Bấm vào nút **Copy policy**.
2. Bấm vào liên kết **Go to S3 bucket permissions** ngay trên thông báo đó để bay thẳng sang S3.
3. Cuộn xuống mục **Bucket policy**, nhấn *Edit*, dán đoạn mã vừa copy vào và nhấn *Save changes*.

AWS sẽ cần từ 3 - 5 phút để phân phối code của bạn ra hàng trăm Edge location trên toàn cầu. Khi trạng thái CloudFront chuyển sang *Deployed*, bạn hãy copy chuỗi URL ở mục *Distribution domain name* (Ví dụ: `d12345abcdef.cloudfront.net`) và mở trên trình duyệt.

![CloudFront App](/fcaj-workshop-template/images/cloudfront-app.png)
