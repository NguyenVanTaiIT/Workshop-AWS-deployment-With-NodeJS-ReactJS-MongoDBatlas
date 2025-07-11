---
title: "Chuẩn bị"
date: 2025-07-07
weight: 2
chapter: true
pre: "<b> 2. </b>"
---

Trong phần này, bạn sẽ hoàn thành tất cả các bước chuẩn bị cần thiết trước khi triển khai và theo dõi ứng dụng trên AWS. Các bài lab này sẽ giúp bạn:

{{< figure src="../../images/2-preparation/001-IAM.png" title="Quản lý danh tính và truy cập (IAM) trên AWS" width=150pc >}}

- Thiết lập tài khoản AWS và bật X-Ray
- Tạo IAM users và roles với quyền phù hợp
- Quản lý credentials an toàn bằng AWS Secrets Manager
- Tích hợp MongoDB Atlas với backend bằng Mongoose

Mỗi lab là nền tảng quan trọng cho một deployment an toàn, có thể quan sát và sẵn sàng cho production.

**Các lab bao gồm:**

- [2.1 Tạo MongoDB Atlas Database](2.1-create-mongodb-atlas-db/)
- [2.2 IAM và Quản lý Secrets](2.2-iam-and-secrets/)
- [2.3 Tạo Elastic Beanstalk Environment](2.3-create-beanstalk/)
- [2.4 Tích hợp MongoDB Atlas](2.4-mongodb-integration/)
- [2.5 Thiết lập S3](2.5-s3-setup/)

Hoàn thành các bước này trước khi chuyển sang instrumentation backend và deployment.
