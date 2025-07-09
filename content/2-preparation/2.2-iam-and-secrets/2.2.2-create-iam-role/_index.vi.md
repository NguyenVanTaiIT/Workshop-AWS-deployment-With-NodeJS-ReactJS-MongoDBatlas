---
title : "Tạo IAM Role cho Beanstalk"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

Tạo một IAM role để gắn vào các EC2 instances của Elastic Beanstalk. Điều này cho phép các instances tương tác với AWS X-Ray, S3 và Secrets Manager.

Các policies cần thiết:
- `AWSXRayFullAccess`
- `AmazonS3FullAccess`
- `SecretsManagerReadWrite`
- `AWSElasticBeanstalkWebTier`

1. Truy cập [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles) để xem hoặc tạo roles cho môi trường của bạn.  


2. Nhấp **Create role** ở đầu trang.
{{< figure src="../../../images/2-preparation/015-IamRole.png" title="Tạo IAM Role mới" >}}


3. Dưới **Trusted entity type**, chọn **AWS service EC2**  
{{< figure src="../../../images/2-preparation/016-IamRole.png" title="Tạo IAM Role mới" >}}

4. Nhấp **Next** để chuyển đến permissions.

---

## 📌 Gắn Policies

5. Trong bước **Permissions**, tìm kiếm và chọn các policies sau:

- `AWSElasticBeanstalkMulticontainerDocker`
- `AWSElasticBeanstalkWebTier`
- `AWSElasticBeanstalkWorkerTier`
- `AWSElasticBeanstalkEnhancedHealth`
- `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`
- `SecretsManagerReadWrite` 
- `AWSXRayDaemonWriteAccess` 
{{< figure src="../../../images/2-preparation/017-IamRole.png" title="Tạo IAM Role mới" >}}
Nhấp **Next**.

---

## 📝 Đặt tên và Tạo Role

6. Nhập tên cho role, ví dụ: `aws-elasticbeanstalk-ec2-role`
7. (Tùy chọn) Thêm mô tả như:  
`IAM role cho Elastic Beanstalk EC2 instances với quyền truy cập X-Ray, Secrets Manager và các tính năng Beanstalk.`
{{< figure src="../../../images/2-preparation/018-IamRole.png" title="Tạo IAM Role mới" >}}
8. Nhấp **Create Role**
9. Sau khi tạo role, quay lại [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles) và tìm kiếm tên role bạn vừa tạo  
(ví dụ: `EcommerceAppInstanceRole`) để xác minh rằng nó xuất hiện trong danh sách.

{{< figure src="../../../images/2-preparation/019-IamRole.png" title="Tạo IAM Role mới" >}}
10. **Gắn Secrets Manager Policy vào IAM Role**

Để cho phép các EC2 instances của Elastic Beanstalk truy xuất an toàn MongoDB credentials từ AWS Secrets Manager, bạn cần gắn một custom policy vào IAM role (ví dụ: `aws-elasticbeanstalk-ec2-role`).

**Các bước:**

1. Truy cập [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles).
2. Nhấp tên role của bạn (ví dụ: `aws-elasticbeanstalk-ec2-role`).
3. Dưới tab **Permissions**, nhấp **Add permissions** → **Create inline policy**.
4. Chọn tab **JSON**, và paste nội dung sau:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "arn:aws:secretsmanager:ap-southeast-1:<your-account-id>:secret:mongodb/connection-*"
    }
  ]
}
```

> 🔁 **Thay thế `<your-account-id>` bằng AWS account ID thực tế của bạn.**

5. Nhấp **Next** → Đặt tên policy (ví dụ: `SecretsManagerAccessPolicy`).
6. Nhấp **Create policy**.

---

### 🔍 Policy này làm gì

- **Cho phép:** `secretsmanager:GetSecretValue`
- **Resource:** Chỉ cho secrets có tên `mongodb/connection-*` trong region `ap-southeast-1`
- **Mục đích:** Cho phép ứng dụng Node.js (trên Beanstalk) lấy MongoDB connection strings an toàn tại runtime bằng AWS SDK.
- **Bảo mật:** Quyền truy cập chỉ giới hạn cho secret cần thiết, tránh phơi bày không cần thiết.

✅ Sau khi gắn, instance Beanstalk của bạn có thể truy cập secrets an toàn — không cần hardcode credentials trong file `.env` hoặc deploy dữ liệu nhạy cảm.

✅ Đảm bảo các policies đã gắn trông đúng.  
Bạn có thể nhấp tên role để xem lại permissions và trust relationships.
