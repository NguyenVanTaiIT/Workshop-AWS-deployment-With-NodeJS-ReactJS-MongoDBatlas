---
title : "Triển khai Backend lên AWS Elastic Beanstalk"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

Trong bước này, **Node.js backend** của bạn giờ đây nên được triển khai lên **AWS Elastic Beanstalk** với:

- AWS X-Ray tracing được bật  
- MongoDB Atlas kết nối an toàn qua Secrets Manager  
- Environment variables được cấu hình đúng  
- Health check và graceful shutdown được implement

✅ Nếu bạn đã hoàn thành setup trong [**Section 2.3.1 – Tạo và Cấu hình Elastic Beanstalk Environment**](../../2-preparation/2.3-create-beanstalk/2.3.1-create-eb-env/), bạn đã sẵn sàng!

---

## 🧪 Xác minh Deployment

Truy cập Elastic Beanstalk environment của bạn (ví dụ):

```bash
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```

Sau đó test các endpoints này:

```http
GET /api/status
GET /health
GET /api/products
```

Bạn nên mong đợi:

- ✅ HTTP 200 responses
- ✅ /health trả về xray: enabled và database: connected
- ✅ Traces xuất hiện trong AWS X-Ray → Service Map

---

## ❗ Chưa triển khai?

Quay lại:
👉 [2.3.1 – Tạo và Cấu hình Elastic Beanstalk Environment](../../2-preparation/2.3-create-beanstalk/2.3.1-create-eb-env/)

Ở đó bạn sẽ học cách:

- Upload backend code đã nén
- Cấu hình environment variables
- Gắn IAM roles
- Bật X-Ray daemon

🎉 Sau khi hoàn thành, bạn có một Node.js backend đầy đủ tính năng chạy trên Elastic Beanstalk với tracing, observability và production readiness.
