---
title : "Dọn dẹp Tài nguyên AWS"
date: 2025-07-07
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

Để tránh **phí AWS liên tục**, điều quan trọng là phải xóa các tài nguyên bạn đã tạo trong quá trình workshop. Bước này đặc biệt quan trọng nếu bạn đang sử dụng **AWS Free Tier**, vì một số dịch vụ (như MongoDB Atlas, EC2, hoặc CloudWatch Logs) có thể phát sinh phí theo thời gian.

---

## 🧹 Tài nguyên cần xóa

Đây là cách dọn dẹp từng dịch vụ:

---

### 1️⃣ Elastic Beanstalk

1. Đi đến [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/)

2. Chọn environment của bạn (ví dụ: `ecommerce-app`)

3. Nhấp **Actions** → **Terminate environment**

   ⚠️ Điều này sẽ chấm dứt EC2, Load Balancer, Auto Scaling, và tài nguyên S3 được tạo bởi Beanstalk.
{{< figure src="../../images/8-cleanup/001-CleanUp.png" title="Xác nhận xóa environment" >}}

4. Xác nhận xóa

{{< figure src="../../images/8-cleanup/002-CleanUp.png" title="Xác nhận xóa environment" >}}

---

### 2️⃣ Amazon S3 Buckets

1. Đi đến [S3 Console](https://s3.console.aws.amazon.com/s3/home)
2. Chọn bucket của bạn (ví dụ: `ecommerce-products-2025`)

{{< figure src="../../images/8-cleanup/003-CleanUp.png" title="Chọn bucket để xóa" >}}

3. **Làm trống** nội dung bucket trước:
   - Nhấp **Empty**

{{< figure src="../../images/8-cleanup/004-CleanUp.png" title="Làm trống bucket" >}}

   - Xác nhận xóa
4. Sau đó nhấp **Delete bucket**

{{< figure src="../../images/8-cleanup/005-CleanUp.png" title="Xóa bucket" >}}

{{< figure src="../../images/8-cleanup/006-CleanUp.png" title="Xác nhận xóa bucket" >}}

{{% notice warning %}}
⚠️ Buckets phải được làm trống trước khi xóa.
{{% /notice %}}

---

### 3️⃣ MongoDB Atlas Cluster

{{% notice note %}}
Nếu bạn đã tạo **MongoDB Atlas cluster**, việc xóa nó sẽ dừng tính phí từ MongoDB (không phải AWS).
{{% /notice %}}

1. Đi đến [MongoDB Atlas](https://cloud.mongodb.com)
2. Điều hướng đến project của bạn

3. Rời khỏi project

{{< figure src="../../images/8-cleanup/007-CleanUp.png" title="Rời khỏi project MongoDB Atlas" >}}

{{< figure src="../../images/8-cleanup/008-CleanUp.png" title="Xác nhận rời khỏi project" >}}

3. Nhấp **Cluster → Terminate**

{{< figure src="../../images/8-cleanup/009-CleanUp.png" title="Terminate cluster MongoDB Atlas" >}}

{{< figure src="../../images/8-cleanup/010-CleanUp.png" title="Xác nhận terminate cluster" >}}

4. Xóa project nếu không sử dụng lại

{{< figure src="../../images/8-cleanup/011-CleanUp.png" title="Xóa project MongoDB Atlas" >}}

---

### 4️⃣ AWS Secrets Manager

1. Đi đến [Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
2. Chọn secret của bạn (ví dụ: `mongodb/connection`, `ecommerce-secrets`)
3. Nhấp **Actions → Delete**
4. Xác nhận xóa

{{< figure src="../../images/8-cleanup/012-CleanUp.png" title="Xóa secret trong AWS Secrets Manager" >}}

Secrets được lên lịch xóa trong 7 ngày (bạn có thể buộc xóa ngay lập tức qua CLI).

---

### 5️⃣ IAM Users và Roles

1. Đi đến [IAM Console](https://console.aws.amazon.com/iam/)
2. Xóa:
   - IAM user (ví dụ: `ecommerce-user`)
   - IAM roles (ví dụ: `EcommerceAppInstanceRole`)
   - IAM policies được tạo cho workshop (nếu tùy chỉnh)

✅ Điều này đảm bảo credentials workshop của bạn không bị lộ hoặc tái sử dụng một cách vô tình.

---

### 6️⃣ CloudWatch Logs (Tùy chọn)

1. Đi đến [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/)
2. Điều hướng đến **Logs → Log groups**
3. Chọn các groups như:
   - `/aws/elasticbeanstalk/...`
   - `/aws/lambda/...` (nếu sử dụng)
4. Nhấp **Actions → Delete log group**

⚠️ CloudWatch logs có thể phát sinh chi phí theo thời gian nếu được giữ lại.

---

### ✅ Lời khuyên cuối cùng

- Chờ vài phút sau khi xóa mọi thứ.
- Đi đến [Billing Console](https://console.aws.amazon.com/billing/home) để kiểm tra **tài nguyên đang hoạt động hoặc chi phí**.

Nếu bạn thấy phí không mong đợi:
- Sử dụng **AWS Cost Explorer**
- Kiểm tra **Resource Groups** hoặc **Trusted Advisor**

---

🎉 **Bạn đã dọn dẹp thành công!**

Cảm ơn bạn đã hoàn thành workshop. Bạn đã xây dựng một ứng dụng AWS full-stack và giờ đây đã đóng tất cả tài nguyên đang hoạt động để ngăn chặn phí.
