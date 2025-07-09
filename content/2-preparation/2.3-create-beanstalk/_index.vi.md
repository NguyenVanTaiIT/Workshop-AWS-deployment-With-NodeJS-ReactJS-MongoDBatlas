---
title : "Tạo Elastic Beanstalk Environment"
date: 2025-07-07
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

## 🚀 AWS Elastic Beanstalk là gì?

{{< figure src="../../../images/2-preparation/048-ElasticBeanstalk.png" title="ElasticBeanstalk" width="150px">}}


**Elastic Beanstalk** là một dịch vụ Platform-as-a-Service (PaaS) từ AWS giúp dễ dàng triển khai, quản lý và mở rộng ứng dụng. Nó hỗ trợ nhiều ngôn ngữ và nền tảng bao gồm **Node.js**, **Java**, **Python** và **.NET**.

Với Elastic Beanstalk, bạn tập trung vào **code**, và AWS xử lý **infrastructure**—như cung cấp EC2 instances, load balancers, auto scaling, monitoring và deployments.

---

### ✅ Lợi ích chính

- **Không cần quản lý infrastructure** — không cần thủ công cung cấp EC2, security groups, hoặc scaling groups
- **Monitoring tích hợp** qua **CloudWatch** và health dashboards
- **Triển khai dễ dàng** qua file zip hoặc Git
- **Tích hợp với IAM, X-Ray, S3** và các dịch vụ AWS khác
- **Hỗ trợ environment variables** và secret injection

---

## 🎯 Trong bước này, bạn sẽ:

- Tải lên **Node.js backend** dưới dạng file zip
- Tạo **Elastic Beanstalk environment**
- Gắn **IAM role** để cho phép truy cập:
  - AWS X-Ray
  - Secrets Manager
  - S3 Buckets
- Bật **X-Ray daemon** bằng `.ebextensions`
- Thiết lập **environment variables** như `JWT_SECRET`, `ALLOWED_ORIGINS`, và `PORT`

Sau khi hoàn thành, ứng dụng backend của bạn sẽ được triển khai đầy đủ trong môi trường AWS production-grade, tự động mở rộng và có thể quan sát được.

---

➡️ Tiếp tục đến [2.3.1 – Triển khai Backend Application](2.3.1-deploy-backend/) để bắt đầu thiết lập.