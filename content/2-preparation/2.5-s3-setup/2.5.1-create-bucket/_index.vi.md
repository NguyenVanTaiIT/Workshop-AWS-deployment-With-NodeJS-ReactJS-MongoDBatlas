---
title: "Tạo S3 Bucket cho Product Images"
date: 2025-07-07
weight: 1
chapter: false
pre: " <b> 2.5.1 </b> "
---

Trong bước này, bạn sẽ tạo một **Amazon S3 bucket mới** để lưu trữ và phục vụ hình ảnh sản phẩm cho ứng dụng thương mại điện tử.

Backend đã được cấu hình sẵn để upload vào bucket có tên: `ecommerce-products-2025`

Hãy đảm bảo bạn tạo bucket với **tên chính xác này** (hoặc cập nhật code backend tương ứng).

---

## 🪣 Từng bước: Tạo S3 Bucket mới

1. Truy cập [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home)

2. Nhấp **Create bucket**
{{< figure src="../../../../images/2-preparation/044-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
3. Điền thông tin sau:

- **Bucket name**: `ecommerce-products-2025`


- **Region**:  
Chọn cùng AWS Region nơi Beanstalk environment của bạn đang chạy (ví dụ: `ap-southeast-1`)

{{< figure src="../../../../images/2-preparation/045-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

---

## 🔐 Bước 2: Tắt Block Public Access

1. Cuộn xuống **Block Public Access settings**

2. Bỏ tick: **Block all public access**

3. Xác nhận cảnh báo xuất hiện, sau đó nhấp **Create bucket**

{{< figure src="../../../../images/2-preparation/046-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

{{< figure src="../../../../images/2-preparation/047-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

---

## 📛 Bước 3: Thêm Bucket Policy (Tùy chọn cho Public Read)

Nếu bạn muốn hình ảnh sản phẩm có thể xem công khai (không cần signed URLs), thêm **Bucket Policy** sau:

1. Chuyển đến bucket vừa tạo  
2. Chọn tab **Permissions**  
3. Nhấp **Edit** dưới **Bucket Policy**, và paste nội dung sau:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUserS3Access",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<your-account-id>:user/<your-username>"
      },
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::ecommerce-products-2025/*"
    },
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::ecommerce-products-2025/*"
    }
  ]
}

```
🔍 Giải thích Permissions
|        SID        |                                        Mục đích                                       
|:-----------------:|:-------------------------------------------------------------------------------------:|
| AllowUserS3Access | Cho phép IAM user cụ thể upload, đọc và xóa objects trong bucket.    
| PublicReadAccess  | Cho phép công chúng (bất kỳ ai) đọc (GET) files trong bucket, như hình ảnh. 



👉 Tiếp tục: [2.5.2 – Test Image Upload to S3](../2.5.2-test-upload/)
