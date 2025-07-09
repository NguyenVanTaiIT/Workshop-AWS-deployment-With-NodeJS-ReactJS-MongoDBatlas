---
title : "Tạo và Sử dụng Secret trong AWS Secrets Manager"
date: 2025-07-07
weight : 3
chapter : false
pre : " <b> 2.2.3 </b> "
---

Trong phần này, bạn sẽ học cách lưu trữ và truy xuất an toàn các credentials nhạy cảm bằng **AWS Secrets Manager**.

Thay vì hardcode MongoDB connection strings hoặc AWS keys trong file `.env` hoặc source code, bạn sẽ sử dụng **AWS Secrets Manager** để lưu trữ secrets và truy cập chúng an toàn từ ứng dụng backend.

---

## 🔐 Từng bước: Tạo secret trong Secrets Manager

1. Truy cập [Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/)

2. Nhấp **Store a new secret**

{{< figure src="../../../../images/2-preparation/040-SecretsManager.png" title="Lưu secret mới trong AWS Secrets Manager" >}}

---

 3. Chọn **Other type of secrets**  
Điều này cho phép bạn định nghĩa các key-value pairs tùy chỉnh một cách thủ công.

---

 4. Trong phần **Plaintext**, nhập nội dung sau (hoặc chỉnh sửa theo nhu cầu):

```json
{
  "MONGODB_URI": "mongodb+srv://<user>:<pass>@cluster.mongodb.net/ecommerce",
  "AWS_ACCESS_KEY": "<your-access-key>",
  "AWS_SECRET_KEY": "<your-secret-key>"
}
```
{{< figure src="../../../../images/2-preparation/041-SecretsManager.png" title="Lưu secret mới trong AWS Secrets Manager" >}}

{{% notice warning %}} 
Thay thế `<user>`, `<password>`, `<your-access-key>`, `<your-secret-key>` bằng credentials thực tế của bạn.
{{% /notice %}}

 5. Nhấp **Next**

 6. Nhập tên cho secret

Ví dụ: `mongodb/connection`


✅ Tên này sẽ được sử dụng trong ứng dụng để truy xuất secret.

Bạn có thể tùy chọn thêm mô tả và tags.

 7. Để **automatic rotation** tắt (hiện tại), sau đó nhấp **Next**

 8. Trên trang **Review**, kiểm tra lại các thông tin đã nhập  
Sau đó nhấp **Store** để tạo secret.

Bạn sẽ thấy thông báo xác nhận và secret mới được liệt kê trong dashboard Secrets Manager.

## 🧪 Xác nhận Secret

Nhấp vào tên secret (`mongodb/connection`) để mở nó.  
Xác minh rằng các key-value pairs đã được lưu đúng.

{{< figure src="../../../../images/2-preparation/042-SecretsManager.png" title="Xem secret đã lưu trong Secrets Manager" >}}

---

## 🧑‍💻 Cách sử dụng secret trong Node.js (runtime access)

Để lấy secret từ ứng dụng backend:

1. Cài đặt AWS SDK v3 nếu chưa có:

```bash
npm install @aws-sdk/client-secrets-manager
```
2. Sử dụng code sau trong backend (ví dụ: server.js hoặc config module):

```js
const { SecretsManagerClient, GetSecretValueCommand } = require('@aws-sdk/client-secrets-manager');

const client = new SecretsManagerClient({ region: 'ap-southeast-1' });

async function getMongoURI() {
  const command = new GetSecretValueCommand({ SecretId: 'mongodb/connection' });
  const data = await client.send(command);
  
  const secret = JSON.parse(data.SecretString);
  return secret.MONGODB_URI;
}
```