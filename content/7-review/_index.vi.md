---
title : "Đánh giá Workshop và Vấn đề thường gặp, Cải tiến tương lai"
date: 2025-07-07
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

## ✅ Những gì bạn đã hoàn thành

Bằng cách hoàn thành workshop này, bạn đã thành công:

- 🚀 Triển khai ứng dụng full-stack **Node.js + React** lên AWS
- 📦 Sử dụng **Elastic Beanstalk** cho deployment và quản lý
- 🧩 Tích hợp **AWS X-Ray** cho end-to-end distributed tracing
- 🔐 Bảo mật credentials sử dụng **AWS Secrets Manager**
- ☁️ Kết nối **MongoDB Atlas** như managed NoSQL database
- 📊 Sử dụng **CloudWatch** cho monitoring cơ bản và health checks
- 🖼️ Bật **image uploads lên Amazon S3**

---

## ⚠️ Vấn đề thường gặp và Troubleshooting

- ❌ **Traces không hiển thị trong X-Ray?**
  - Đảm bảo `.ebextensions/xray.config` được deploy
  - Kiểm tra IAM Role có `AWSXRayFullAccess`
  - Xác minh `AWS_XRAY_DAEMON_ADDRESS` được set thành `127.0.0.1:2000`

- ❌ **MongoDB connection thất bại?**
  - Đảm bảo secret name tồn tại trong **Secrets Manager**
  - Đảm bảo IP hiện tại hoặc EC2 security group được whitelist trong MongoDB Atlas

- ❌ **CORS hoặc cookies không hoạt động?**
  - Kiểm tra lại environment variable `ALLOWED_ORIGINS`
  - Đảm bảo frontend và backend được serve từ domains đúng

- ❌ **S3 upload issues?**
  - Xác nhận CORS settings và IAM permissions đúng cho S3 bucket
  - Kiểm tra uploaded files có thể truy cập công khai (qua S3 Bucket Policy)

---

## 🚧 Cải tiến tương lai 

Mặc dù phiên bản workshop này cung cấp complete deployment pipeline, đây là một số **tính năng đã lên kế hoạch** để cải tiến thêm:

- 📜 **Bật AWS CloudTrail**  
  Capture API activity logs cho auditing và visibility qua các services.

- 📈 **Tích hợp Auto Scaling Groups**  
  Tự động scale EC2 instances dựa trên traffic hoặc CPU usage sử dụng Elastic Beanstalk's scaling options.

- 🔒 **Advanced secrets rotation**  
  Bật automatic rotation của secrets cho MongoDB hoặc AWS credentials.

- 📉 **Sử dụng Amazon CloudWatch Alarms**  
  Alert khi memory cao, CPU, hoặc errors per minute theo thời gian thực.

- 🧪 **Tích hợp CI/CD với CodePipeline + CodeBuild**  
  Tự động hóa build và deployment pipeline cho mỗi code push lên GitHub.

- 🛰️ **Bật VPC private subnets**  
  Cải thiện bảo mật bằng cách chạy databases và internal services trong private subnets.

- 🌐 **Sử dụng Route 53 với HTTPS + Custom Domain**  
  Map Beanstalk environments hoặc CloudFront distributions đến domain riêng với SSL certificates.

---

🎉 **Chúc mừng!**  
Bạn đã triển khai một ứng dụng full-stack production-ready với AWS observability, secret management, và database connectivity.
