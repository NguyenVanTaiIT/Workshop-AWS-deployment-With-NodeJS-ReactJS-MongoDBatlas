---
title : "Tích hợp MongoDB Atlas và Mongoose"
date: 2025-07-07
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

Trong phần này, bạn sẽ xác minh cách ứng dụng backend kết nối an toàn với **MongoDB Atlas** bằng **Mongoose** và AWS **Secrets Manager**.

Bạn không cần viết logic từ đầu. Tích hợp hoàn chỉnh đã được implement trong file `server.js` có sẵn trong source code workshop.

---

## 📦 Tải xuống Source Code

👉 [Tải xuống Workshop Code (.zip)](https://your-link.com/aws-xray-workshop-src.zip)

Giải nén và mở thư mục `backend/`. Tất cả logic kết nối database nằm trong `server.js`.

---

## 🧠 Cách hoạt động

- AWS Secrets Manager lưu trữ MongoDB connection string dưới key `mongodb/connection`
- Tại runtime, ứng dụng lấy giá trị đó bằng AWS SDK:

```js
const client = new SecretsManagerClient({ region: 'ap-southeast-1' });
const command = new GetSecretValueCommand({ SecretId: 'mongodb/connection' });
const data = await client.send(command);
const uri = JSON.parse(data.SecretString).MONGODB_URI;
```
Sau đó kết nối MongoDB bằng:

```js
await mongoose.connect(uri, { ...connectionOptions });
```

---

## 🔍 Những gì cần tìm
Mở function này trong server.js:

```js
const connectMongoDB = async (maxRetries = 5, retryDelay = 3000) => { ... }
```
Bao gồm:

- Logic auto-retry connection
- Cài đặt timeout
- Pool sizing
- Xử lý lỗi graceful

---

## 🧪 Xác minh kết nối
Sau khi triển khai backend:

- Truy cập endpoint /health
- Bạn sẽ thấy:

```json
"services": {
  "database": "connected",
  "xray": "enabled"
}
```
Nếu MongoDB bị ngắt kết nối hoặc credentials sai, status sẽ hiển thị `"database": "disconnected"`.

---

## 🛡️ Nhắc nhở bảo mật
- MongoDB credentials không được hardcode
- Secrets được lưu trong AWS Secrets Manager
- Chỉ Beanstalk EC2 role mới có thể truy cập

✅ Với setup này, backend của bạn được kết nối an toàn và đáng tin cậy với MongoDB Atlas theo cách production-ready.