---
title : "Cập nhật Cấu hình Elastic Beanstalk Environment"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 2.3.2 </b> "
---

Trong bước này, bạn sẽ **xem lại và cập nhật các cài đặt cấu hình** cho Elastic Beanstalk environment đã tạo trước đó.

Bao gồm:

- Thiết lập environment variables
- Đảm bảo IAM role được gắn
- Bật AWS X-Ray daemon
- Xác minh quyền truy cập security group và sức khỏe instance

---

 🔧 Bước 1: Mở Environment Settings

1. Truy cập [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/)
2. Chọn environment của bạn (ví dụ: `ecommerce-env`)
3. Nhấp **Configuration**

---

 🧪 Bước 2: Chỉnh sửa Software Settings

Nhấp **Edit** trong phần **Software**:

Thêm hoặc cập nhật các environment variables này:

| Key                       | Value                                                |
|---------------------------|------------------------------------------------------|
| `PORT`                   | `8080`                                               |
| `NODE_ENV`               | `production`                                         |
| `AWS_XRAY_DAEMON_ADDRESS`| `127.0.0.1:2000`                                     |
| `JWT_SECRET`             | *(JWT secret tùy chỉnh của bạn)*                    |
| `REFRESH_TOKEN_SECRET`   | *(refresh token secret của bạn)*                    |
| `ALLOWED_ORIGINS`        | `http://localhost:5173,https://your-frontend.com`    |

✅ Đảm bảo `PORT` khớp với port mà Node.js server của bạn lắng nghe (mặc định là `8080`).

---

 🔐 Bước 3: Gắn IAM Instance Profile

Nhấp **Edit** trong phần **Security**:

- Đặt **EC2 instance profile** thành:  
  `EcommerceAppInstanceRole`  
  *(Đã tạo trước đó với quyền X-Ray và Secrets Manager)*

Điều này đảm bảo ứng dụng của bạn có thể:

- Đọc secrets từ AWS Secrets Manager
- Gửi dữ liệu trace đến AWS X-Ray
- Truy cập S3 nếu cần

---

 ⚙️ Bước 4: Đảm bảo X-Ray Daemon Đang Chạy

Đảm bảo `ecommerce-backend.zip` của bạn chứa:

File `xray.config`

File này cấu hình EC2 instance để cài đặt và khởi động AWS X-Ray daemon trên port `2000`.

Mẫu `.ebextensions/xray.config`:

```yaml
files:
  "/etc/xray-daemon.cfg":
    mode: "000644"
    owner: root
    group: root
    content: |
      {
        "Daemon": {
          "BindAddress": "127.0.0.1:2000",
          "Region": "ap-southeast-1"
        }
      }

services:
  sysvinit:
    xray:
      enabled: true
      ensureRunning: true
      files:
        - /etc/xray-daemon.cfg
```

---

 ✅ Bước 5: Triển khai lại (nếu cần)
Nếu bạn đã thay đổi environment settings hoặc package zip:

- Chuyển đến Elastic Beanstalk > Application versions
- Upload và triển khai file .zip đã cập nhật
- Chờ trạng thái sức khỏe xanh

🔍 Xác nhận mọi thứ đang hoạt động:
- Truy cập endpoint /health → nên hiển thị xray: enabled
- Chuyển đến AWS X-Ray Console > Service Map → nên hiển thị backend service của bạn
- Kiểm tra Logs > Request logs trong Beanstalk nếu có vấn đề
