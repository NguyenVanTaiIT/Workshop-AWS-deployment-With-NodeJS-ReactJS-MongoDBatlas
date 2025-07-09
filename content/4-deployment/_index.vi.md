---
title: "Triển khai Backend và Frontend"
date: 2025-07-07
weight: 4
chapter: true
pre: "<b> 4. </b>"
---

Trong chương này, bạn sẽ triển khai cả **backend (Node.js + MongoDB + AWS X-Ray)** và **frontend (React + Vite)** của ứng dụng thương mại điện tử lên AWS infrastructure.

Chúng ta sẽ bao gồm:

### 📌 4.1 Triển khai Backend lên Elastic Beanstalk

- Triển khai Node.js backend lên AWS Elastic Beanstalk
- Sử dụng code đã được cấu hình sẵn với:
  - AWS X-Ray tracing
  - MongoDB Atlas integration
  - Secrets Manager
  - Auto health checks và logging
- Xác minh deployment với `/health` và `/api/status`
- Đảm bảo traces hiển thị trong AWS X-Ray Console

👉 Xem: [4.1 Triển khai Backend lên AWS Elastic Beanstalk](4.1-deploy-backend/)

---

### 📌 4.2 Triển khai React Frontend lên AWS

- Build React Vite frontend
- Triển khai sử dụng:
  - Option A: Amazon S3 + CloudFront (static hosting)
  - Option B: Second Elastic Beanstalk environment (tùy chọn)
- Cấu hình frontend để giao tiếp với backend an toàn
- Trigger end-to-end traces từ frontend → backend → database

👉 Xem: [4.2 Triển khai React Frontend lên AWS (S3/CloudFront hoặc Beanstalk)](4.2-deploy-frontend/)

---

Sau khi hoàn thành chương này, bạn sẽ có **toàn bộ ứng dụng MERN stack** được triển khai đầy đủ trên AWS với **production-grade observability** sử dụng **X-Ray**, **CloudWatch**, và **Secrets Manager**.
