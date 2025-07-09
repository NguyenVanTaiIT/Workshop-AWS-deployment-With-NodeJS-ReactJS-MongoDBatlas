---
title : "Triển khai React Frontend lên AWS Elastic Beanstalk"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

Trong phần này, bạn sẽ triển khai **React frontend** của ứng dụng thương mại điện tử bằng cách kết hợp nó vào cùng deployment với Node.js backend sử dụng **Elastic Beanstalk**.

Khác với S3/CloudFront hosting, cách tiếp cận này sử dụng Express backend để serve frontend static files (`dist/`)—cho phép routing liền mạch, cookies và CORS dưới cùng domain.

---

## 📦 Tải xuống Frontend Source Code

[⬇️ Tải xuống Frontend Source (frontend.zip)](./../../../downloads/ecommerce-frontend.zip)

Sau khi tải xuống:

1. Giải nén archive  
2. Trong thư mục đã giải nén (`frontend/`), chuyển đến `src/`  
3. Tạo file tên `.env.production` với nội dung sau:

```env
VITE_API_URL=https://your-backend-env.ap-southeast-1.elasticbeanstalk.com/api
```
Thay thế URL bằng backend Beanstalk environment thực tế của bạn.

---

⚙️ **Build Frontend**
Mở terminal trong thư mục frontend/

Chạy các lệnh sau:

```bash
npm install
npm run build
```
Điều này sẽ tạo ra thư mục `dist` chứa static assets production-ready.

---

🚀 **Di chuyển dist/ vào Backend và Triển khai**
Copy thư mục dist/ vừa tạo

Paste vào `backend/ directory` (bên cạnh server.js)

Cấu trúc backend của bạn giờ nên như sau:

```bash
backend/
├── server.js
├── dist/
│   ├── index.html
│   └── assets/
├── routes/
├── .ebextensions/
└── ...
```

Nén lại thư mục backend:

```bash
cd backend
zip -r ../ecommerce-backend-with-frontend.zip .
```

Truy cập Elastic Beanstalk Console

- Chọn environment ecommerce-app hiện có



{{< figure src="../../../images/4-deployment/001-FrontendDeploy.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
- Nhấp Upload and deploy
{{< figure src="../../../images/4-deployment/002-FrontendDeploy.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
- Chọn file zip mới: **ecommerce-backend-with-frontend.zip** và label `version-2`
{{< figure src="../../../images/4-deployment/003-FrontendDeploy.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
- Nhấp **Deploy**
---

🌐 **Truy cập Ứng dụng**
Truy cập URL Beanstalk environment:

```http
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```
Bạn sẽ thấy React frontend được triển khai — tích hợp hoàn toàn với backend.

{{< figure src="../../../images/4-deployment/004-FrontendDeploy.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

---

{{% notice note %}}
Code sau trong server.js đảm bảo static frontend routing hoạt động
{{% /notice %}}


```js
app.use(express.static(path.join(__dirname, 'dist')));

app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'dist', 'index.html'));
});
```

🎉 Ứng dụng thương mại điện tử full-stack của bạn giờ đã được triển khai lên AWS Elastic Beanstalk — hoàn chỉnh với frontend, backend, API và X-Ray observability.


