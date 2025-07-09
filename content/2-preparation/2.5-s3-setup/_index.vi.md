---
title: "Thiết lập Amazon S3 cho Image Upload"
date: 2025-07-07
weight: 5
chapter: false
pre: " <b> 2.5. </b> "
---

# ☁️ Tổng quan Amazon S3
{{< figure src="./../../images/2-preparation/043-S3.png" title="Amazon Simple Storage Service (Amazon S3)" width="150">}}
**Amazon S3 (Simple Storage Service)** là dịch vụ lưu trữ object có thể mở rộng, bền vững và có tính khả dụng cao được cung cấp bởi AWS. Nó cho phép developers lưu trữ và truy xuất bất kỳ lượng dữ liệu nào vào bất kỳ lúc nào, từ bất kỳ đâu trên web.

### 💡 Các trường hợp sử dụng phổ biến

- Hosting các file tĩnh như HTML, CSS, JS
- Lưu trữ hình ảnh, video hoặc tài liệu được upload bởi user
- Phục vụ nội dung có thể tải xuống
- Hoạt động như backup hoặc archive storage
- Hỗ trợ data lakes và machine learning workflows

---

## 🛍️ S3 làm gì trong Workshop này

Trong workshop thương mại điện tử này, Amazon S3 được sử dụng để:

- Lưu trữ **hình ảnh sản phẩm** được upload
- Phục vụ hình ảnh qua **public URLs**
- Tích hợp an toàn với backend bằng **IAM và AWS SDK**
- Log và trace hoạt động upload bằng **AWS X-Ray**

Backend của bạn đã được cấu hình sẵn để upload hình ảnh sản phẩm vào S3 bucket được chỉ định (ví dụ: `ecommerce-products-2025`) và trả về URLs có thể truy cập để hiển thị trong UI.

---

## ✅ Những gì bạn sẽ làm trong phần này

- Tạo S3 bucket mới
- Thiết lập permissions và **public access**
- Cấu hình **CORS policy** để cho phép upload từ frontend
- Upload hình ảnh từ admin dashboard
- Xác minh hình ảnh có thể truy cập online
- Xác nhận hoạt động upload **có thể trace trong AWS X-Ray**

---

## 📁 Source Code liên quan

Chức năng này được xử lý trong backend bởi:

```bash
📦 backend/
└── controllers/productController.js
└── uploadProductImage() // S3 + X-Ray integration
``` 
    
Bạn có thể test qua:

```http
POST /api/products/upload
```

👉 Tiếp tục bước tiếp theo: [Tạo S3 Bucket cho Product Images](./2.5.1-create-bucket/)
