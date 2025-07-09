---
title : "Giới thiệu"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

Workshop này giới thiệu về việc theo dõi và giám sát end-to-end của ứng dụng thương mại điện tử Node.js + React sử dụng các dịch vụ AWS như X-Ray, CloudWatch, Secrets Manager và Elastic Beanstalk. Bạn sẽ học cách theo dõi các yêu cầu của người dùng qua các dịch vụ, trực quan hóa các điểm nghẽn về độ trễ, gỡ lỗi logic backend và quản lý bảo mật các bí mật ứng dụng—tất cả trong AWS.

Chúng ta sẽ sử dụng **MongoDB Atlas** để lưu trữ cơ sở dữ liệu, **Amazon S3** để tải lên hình ảnh và **Elastic Beanstalk** để triển khai. Frontend (React Vite) và backend (Node.js với Express) được tích hợp với **AWS X-Ray SDK**, cung cấp khả năng hiển thị sâu vào hiệu suất ứng dụng và tương tác dịch vụ.

### Những lợi ích chính bạn sẽ đạt được từ workshop này:
- Hiểu cách AWS X-Ray theo dõi các yêu cầu qua các API backend và thao tác cơ sở dữ liệu.
- Học cách quản lý bí mật môi trường một cách an toàn bằng AWS Secrets Manager.
- Tự động hóa triển khai bằng Elastic Beanstalk và giám sát các chỉ số sức khỏe qua CloudWatch.
- Trực quan hóa traces và gỡ lỗi nhanh chóng bằng AWS Application Signals.
- Thay thế logging thủ công truyền thống bằng distributed tracing có cấu trúc.
- Khám phá các ví dụ thực tế: danh sách sản phẩm, tải lên hình ảnh, đặt hàng và xem traces của chúng theo thời gian thực.

Đến cuối workshop, bạn sẽ xây dựng và triển khai một ứng dụng thương mại điện tử full-stack với các phương pháp tốt nhất về tracing và monitoring—sẵn sàng để mở rộng và sản xuất.
