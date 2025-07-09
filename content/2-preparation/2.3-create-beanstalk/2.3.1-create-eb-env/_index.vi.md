---
title : "Tạo và Cấu hình Elastic Beanstalk Environment"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 2.3.1 </b> "
---

Trong bước này, bạn sẽ tạo và cấu hình một **Elastic Beanstalk environment** để host Node.js backend.

Bạn sẽ:

- Tải lên source code backend dưới dạng zip
- Thiết lập environment variables
- Gắn IAM instance role
- Bật AWS X-Ray daemon cho tracing
- Triển khai và xác minh ứng dụng backend

---

## 📦 Bước 1: Chuẩn bị Source Code Backend

Tải xuống source code backend (đã được cấu hình cho Beanstalk + X-Ray):

[⬇️ Tải xuống Workshop backend Source Code(backend.zip)](../../../downloads/ecommerce-backend.zip)

Sau khi tải xuống, giải nén và nén lại nội dung của thư mục `backend/` (không phải thư mục chính):

```bash
cd backend
zip -r ../ecommerce-backend.zip .
```

---

## 🔐 Bước 2: Chuẩn bị Environment Variables
Sử dụng template .env này cho development local hoặc áp dụng như environment variables trong Beanstalk:

```ini
PORT=8080
ALLOWED_ORIGINS=http://your-backend-url.elasticbeanstalk.com,http://localhost:5173

JWT_SECRET=your-jwt-secret-key
REFRESH_TOKEN_SECRET=your-refresh-token-secret-key

SECRET_NAME=your-secret-name
AWS_REGION=ap-southeast-1
NODE_ENV=development

ENABLE_XRAY=true
XRAY_DAEMON_ADDRESS=127.0.0.1:2000

MONGODB_URI=mongodb+srv://your-username:your-password@cluster0.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0

AWS_ACCESS_KEY=your-aws-access-key
AWS_SECRET_KEY=your-aws-secret-key
```

⚠️ Lưu trữ các key nhạy cảm (ví dụ: AWS & MongoDB credentials) an toàn trong AWS Secrets Manager.

---

## ☁️ Bước 3: Tạo Elastic Beanstalk Environment
Truy cập [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/home)

{{< figure src="../../../images/2-preparation/033-ElasticBeanstalk.png" title="Elastic Beanstalk Console" >}}

- Nhấp **Create Application**
- Điền thông tin:
  - Application name: `ecommerce-app`
- Dưới Platform:
  - Platform: Node.js
  - Platform Branch: Node.js 20 running on 64bit Amazon Linux 2023
- Dưới Application code:
  - Chọn Upload your code
  - Upload: **ecommerce-backend.zip**

{{< figure src="../../../images/2-preparation/034-ElasticBeanstalk.png" title="Chọn Platform và Upload Application" >}}
{{< figure src="../../../images/2-preparation/035-ElasticBeanstalk.png" title="Environment Type và Upload ZIP" >}}

---

## ⚙️ Bước 4: Cấu hình Service Access

{{< figure src="../../../images/2-preparation/036-ElasticBeanstalk.png" title="Gắn IAM Role vào EC2 instance" >}}

{{% notice note %}}
IAM role này cho phép EC2 instance truy cập Secrets Manager, S3 và X-Ray
{{% /notice %}}

---

## ⚙️ Bước 5: Bật Monitoring và X-Ray
Dưới Monitoring **bật AWS X-Ray tracing**

{{< figure src="../../../images/2-preparation/037-ElasticBeanstalk.png" title="Bật X-Ray daemon trong environment settings" >}}

Nhấp Apply.

Sau đó, xem lại tất cả settings, rồi **nhấp Create environment** 

---

## ⚙️ Bước 6: Thiết lập Environment Variables
Sau khi environment đã chạy:

- Chuyển đến tab Configuration
- Nhấp **Edit**

{{< figure src="../../../images/2-preparation/038-ElasticBeanstalk.png" title="Chỉnh sửa software settings để thêm environment variables" >}}

Copy các variables sau:

```ini
PORT=8080
NODE_ENV=production
AWS_XRAY_DAEMON_ADDRESS=127.0.0.1:2000
JWT_SECRET=your-jwt-secret-key
REFRESH_TOKEN_SECRET=your-refresh-token-secret-key
ALLOWED_ORIGINS=http://localhost:5173,https://your-frontend
```

Nhấp **Add environment property**.
{{< figure src="../../../images/2-preparation/039-ElasticBeanstalk.png" title="Thiết lập environment variables" >}}

Nhấp Apply để lưu thay đổi.

{{% notice tip %}}
💡 Bạn cũng có thể thêm variables như SECRET_NAME, AWS_ACCESS_KEY, và AWS_SECRET_KEY nếu ứng dụng chưa fetch từ Secrets Manager.
{{% /notice%}}

---

## ✅ Bước 7: Xác minh Deployment
Sau khi triển khai, truy cập URL environment (ví dụ):

```arduino
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```

Test các endpoints sau:

```http
GET /api/status
GET /health
GET /api/products
```

### 🧪 Xác minh:
- ✅ API trả về 200 OK
- ✅ /health trả về:
  - database: connected
  - xray: enabled
- ✅ Traces xuất hiện trong AWS X-Ray > Service Map

🧩 Troubleshooting
- ❌ App crash hoặc màn hình trắng?
  → Kiểm tra Logs → Last 100 lines trong EB console
- ❌ MongoDB không kết nối?
  → Kiểm tra secret mongodb/connection có tồn tại và IAM role có permission
- ❌ Không có traces?
  → Đảm bảo .ebextensions/01_xray.config tồn tại và IAM role có AWSXRayFullAccess

🎉 **Chúc mừng!**
Bạn đã thành công triển khai backend production-ready với AWS Elastic Beanstalk, kết nối MongoDB Atlas, tích hợp AWS X-Ray và sẵn sàng để mở rộng.
