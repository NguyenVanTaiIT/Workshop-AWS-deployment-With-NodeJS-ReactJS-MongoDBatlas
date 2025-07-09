---
title: "Test Image Upload to S3"
date: 2025-07-07
weight: 2
chapter: false
pre: " <b> 2.5.2 </b> "
---

Sau khi tạo S3 bucket và cấu hình CORS, đã đến lúc **test image uploads** thông qua giao diện admin của ứng dụng đã triển khai.

Backend của bạn đã được cấu hình sẵn để chấp nhận file hình ảnh và lưu trữ chúng trong bucket path đúng bằng route `uploadProductImage`.

---

## ✅ Những gì bạn sẽ xác minh

- Frontend có thể upload file hình ảnh bằng admin panel
- Image URLs được lưu trong S3 dưới prefix `products/`
- URLs có thể truy cập công khai (dựa trên bucket policy)

---

## 🧪 Từng bước: Test Upload

1. Truy cập **admin dashboard**
   - URL: `http://your-backend-env.elasticbeanstalk.com`
2. Chuyển đến phần **Product Management** hoặc **Add Product**
3. Điền các trường bắt buộc như:
   - Product Name
   - Price
   - Category
   - Brand
   - SKU
4. Nhấp nút **Upload Image**
5. Chọn file hình ảnh (ví dụ: `.jpg`, `.png`)
6. Chờ upload hoàn thành (bạn sẽ thấy preview hoặc tên file)
7. Nhấp **Save** hoặc **Create Product**

---

## 🔍 Kết quả mong đợi

Sau khi upload thành công:

- ✅ Hình ảnh được upload vào S3 bucket (ví dụ: `ecommerce-products-2025`)
- ✅ Product entry sẽ lưu image URL như:

```text
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/products/1699355589999-laptop.jpg
```

✅ Bạn có thể copy và paste URL vào browser để xem hình ảnh

✅ Không có lỗi CORS trong browser console

🧰 Đằng sau hậu trường
Image upload của bạn sẽ hit endpoint backend này:

```http
POST /api/products/upload
Content-Type: multipart/form-data
Authorization: Bearer <admin-token>
```

Được xử lý bởi:

routes/products.js

controllers/productController.js → uploadProductImage()

Nó sử dụng AWS SDK v3 và X-Ray tracing để log chi tiết hành vi upload.

🧩 Troubleshooting:

|          Vấn đề          |                             Giải pháp                             |
|:-------------------------:|:----------------------------------------------------------------:|
| ❌ Hình ảnh không hiển thị       | Đảm bảo tên bucket & region đúng trong code                      |
| ❌ Access Denied           | Xác nhận IAM Role có AmazonS3FullAccess                          |
| ❌ Image URL 403 Forbidden | Đảm bảo bucket policy cho phép s3:GetObject cho path đúng |

🎉 Chúc mừng!
Bạn đã thành công hoàn thành tích hợp S3 image upload và xác nhận rằng cấu hình frontend + backend + bucket hoạt động end-to-end.
