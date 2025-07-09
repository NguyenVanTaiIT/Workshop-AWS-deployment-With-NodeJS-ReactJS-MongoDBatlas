---
title : "Bật S3 Upload và Image Hosting"
date: 2025-07-07
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

Trong phần này, bạn sẽ **xác minh và test image uploading** sử dụng **Amazon S3**, tích hợp trực tiếp vào backend API.

Backend của bạn đã được cấu hình sẵn với:
- **AWS SDK v3 (`@aws-sdk/client-s3`)**
- **Multer** để xử lý form-data uploads
- **X-Ray tracing** cho upload activity
- Logic để tạo và trả về **public URLs** từ S3 bucket

---

## ✅ Những gì bạn sẽ làm

- Xác nhận S3 bucket (ví dụ: `ecommerce-products-2025`) được tạo đúng
- Upload product images từ **admin dashboard**
- Xác minh image URLs có thể truy cập từ **Amazon S3**
- Xác nhận tracing xuất hiện trong **AWS X-Ray**

---

## 🔍 Cách Upload hoạt động

Backend route chịu trách nhiệm cho file uploads là:

```js
POST /api/products/upload
```
Nó sử dụng uploadProductImage controller trong controllers/productController.js.

Function này làm những việc sau:

Parse file sử dụng multer:

```js
const upload = multer();
```
Chấp nhận multipart file như image field

Gửi nó như buffer đến S3

Khởi tạo S3 client trong AWS region của bạn, sử dụng environment variables:

```js
const s3Client = new S3Client({
  region: process.env.AWS_REGION || 'ap-southeast-1',
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY,
    secretAccessKey: process.env.AWS_SECRET_KEY
  }
});
```
Build file path và upload file:

```js
const params = {
  Bucket: 'ecommerce-products-2025', // Đảm bảo khớp với tên bucket của bạn!
  Key: `products/${Date.now()}-${req.file.originalname}`,
  Body: req.file.buffer,
  ContentType: req.file.mimetype
};

await s3Client.send(new PutObjectCommand(params));
```
Trả về S3 URL như:

```bash
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/products/your-image.jpg
```

---

## 🧪 Test Upload

1. Đăng nhập vào app như Admin
2. Chuyển đến Admin Dashboard → Add Product
3. Chọn image và nhấp Upload
4. Submit form
5. Sau khi lưu, bạn sẽ thấy uploaded image xuất hiện bên cạnh product.

Image URL nên bắt đầu với:

```bash
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/...
```
✅ Bạn có thể nhấp link này và xem công khai.

---

## ⚙️ Những gì cần đúng

✅ **Bucket Name trong productController.js:**
Đảm bảo dòng này:

```js
Bucket: 'ecommerce-products-2025'
```
khớp với tên S3 bucket thực tế của bạn (cái bạn tạo thủ công hoặc trong lab step [2.5.1 - Tạo S3 Bucket](../../2.5.1-create-bucket/)).

✅ **IAM Role được sử dụng bởi Beanstalk phải có policy này:**

- AmazonS3FullAccess

Hoặc custom policy cho phép s3:PutObject, s3:GetObject cho bucket của bạn

✅ **CORS Settings trong S3 bucket:**
Đảm bảo bạn đã cấu hình:

```xml
<CORSRule>
  <AllowedOrigin>*</AllowedOrigin>
  <AllowedMethod>GET</AllowedMethod>
  <AllowedMethod>POST</AllowedMethod>
  <AllowedMethod>PUT</AllowedMethod>
  <AllowedHeader>*</AllowedHeader>
</CORSRule>
```

---

## 📦 Sample Environment Variables
Các environment variables này phải có trong Elastic Beanstalk config hoặc .env:

```env
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY=your-access-key
AWS_SECRET_KEY=your-secret-key
```
Không có những thứ này, S3Client sẽ không authenticate đúng và uploads sẽ thất bại.

---

## 🔎 X-Ray Tracing
Mỗi file upload được trace qua AWS X-Ray:

**FileValidation segment logs:**
- file size
- mimetype
{{< figure src="./../images/5-s3-upload/001-S3uploads.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
**S3Upload segment logs:**
- image_url
- key
- upload result hoặc error
{{< figure src="./../images/5-s3-upload/002-S3uploads.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
Truy cập X-Ray Console và navigate đến Service Map để xác nhận upload activity.

---

🎉 Sau khi xác minh, bạn giờ có complete S3 image hosting integration với traceable uploads, public file access, và admin-managed product creation.
