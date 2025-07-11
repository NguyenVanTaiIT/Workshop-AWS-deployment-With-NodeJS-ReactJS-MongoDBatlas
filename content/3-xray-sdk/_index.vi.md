---
title : "Instrument Node.js Backend với AWS X-Ray SDK"
date: 2025-07-07
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

Trong phần này, bạn sẽ khám phá cách **AWS X-Ray SDK** được tích hợp đầy đủ vào Node.js backend để cung cấp tracing cho:

- Incoming HTTP requests
- MongoDB queries
- Internal business logic
- Errors and health diagnostics

Tất cả code cần thiết đã được implement trong `server.js`. Bước này là về **hiểu, xác minh và test** setup tracing.

📥 [Tải X-Ray Daemon tại đây](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html)

---

## 📂 Vị trí source code

- File: `backend/server.js`
- Module: [aws-xray-sdk](https://www.npmjs.com/package/aws-xray-sdk)

---

## 🧠 Các tính năng chính của Integration

### ✅ 1. Auto-tracing của HTTP requests

```js
const AWSXRay = require('aws-xray-sdk');
AWSXRay.captureHTTPsGlobal(require('http'), true);
AWSXRay.setDaemonAddress('127.0.0.1:2000');
app.use(AWSXRay.express.openSegment('EcommerceApp'));
```
Tự động trace tất cả incoming Express routes.
Capture upstream headers (từ frontend hoặc ALB).
Gửi segments đến local X-Ray daemon tại 127.0.0.1:2000 nếu chạy local.

### ✅ 2. MongoDB Tracing qua Custom Subsegments

```js
const segment = AWSXRay.getSegment();
const subsegment = segment.addNewSubsegment('MongoDB - Connect');
// ...
await mongoose.connect(uri, options);
subsegment.close();
```
Pattern này cho phép bạn trace database-specific logic. Nó được sử dụng trong function connectMongoDB().

### ✅ 3. CORS Headers & Trace Propagation

```js
allowedHeaders: ['Content-Type', 'Authorization', 'X-Amzn-Trace-Id', 'x-amz-security-token'],
exposedHeaders: ['Set-Cookie', 'X-Amzn-Trace-Id']
```
Setup này đảm bảo full trace propagation qua frontend → backend → AWS services.

### ✅ 4. Structured Logging với Trace ID

```js
const traceId = req.headers['x-amzn-trace-id'] || 'local';
console.log(`[${traceId}] ${req.method} ${req.originalUrl}`, { headers, body });
```
Cho phép correlate logs với traces trong X-Ray console.

### ✅ 5. X-Ray Segment Closure

```js
app.use(AWSXRay.express.closeSegment());
```
Điều này cần thiết để finalize mỗi trace, và nên được sử dụng sau khi tất cả routes được register.

---

### 🩺 Health Check + X-Ray Visibility
Endpoint /health cũng kiểm tra:

- MongoDB connectivity qua ping()
- X-Ray daemon address availability

Sample output:

```json
{
  "status": "healthy",
  "services": {
    "database": "connected",
    "xray": "enabled"
  }
  // ...
}
```

---

## 🧪 Xác minh trong AWS Console
Mở X-Ray Console → Service Map

Tìm EcommerceApp

Inspect traces cho:

- Latency
- MongoDB errors
- End-to-end flows từ frontend

---

## ✅ Tóm tắt
- Backend của bạn được instrument với X-Ray.
- Traces capture chi tiết API + DB behavior.
- Logs được correlate với trace IDs.
- An toàn và production-ready.

---

## 🎯 Chi tiết Tracing với Annotation và Metadata
Trong backend project này, mỗi API operation chính được wrap bằng helper function thêm X-Ray subsegments, với detailed annotations và metadata để enhance observability.

### 📍 Vị trí trong Source Code
- File: `controllers/productController.js`
- Wrapper Function: `withXRay(segmentName, fn)`

### 🧩 Annotation và Metadata là gì?
- **Annotations** là indexed key–value pairs — được sử dụng để filter trong X-Ray console.
- **Metadata** là key–value data (bất kỳ JSON type nào) — được sử dụng cho detailed trace context.

### ✅ Ví dụ trong Code
#### getProducts API
Khi users xem product list, system capture:

```js
segment.addAnnotation('category', category || 'all');
segment.addAnnotation('page', parseInt(page));
segment.addMetadata('query_params', { category, brand, sort, page, limit });
segment.addMetadata('category_counts', categoryCounts);
```
→ Điều này giúp bạn trace:
- Filters nào được sử dụng
- Bao nhiêu products được tìm thấy
- Categories nào có mặt

#### uploadProductImage API
Khi admins upload product image lên S3:

```js
s3Segment.addAnnotation('upload_success', true);
s3Segment.addAnnotation('image_url', imageUrl);
s3Segment.addMetadata('upload_result', {
  imageUrl,
  key: params.Key,
  bucket: params.Bucket
});
```
→ Bạn có thể inspect uploaded image details hoặc trace failures (ví dụ: S3 permission issues).

#### createProduct API
Trong quá trình tạo product mới:

```js
dbSegment.addAnnotation('product_created', true);
dbSegment.addMetadata('product_info', {
  id: product._id,
  name,
  sku,
  image,
  price,
  category
});
```
→ Cho phép bạn trace product nào được thêm và liệu việc tạo có thành công hay thất bại.

### ⚠️ Error Tracing
Trong trường hợp thất bại, những điều sau được thêm:

```js
segment.addAnnotation('status', 'error');
segment.addAnnotation('error_type', err.name);
segment.addMetadata('error_stack', err.stack);
segment.addMetadata('error_context', {
  operation: segmentName,
  timestamp: new Date().toISOString(),
  args_count: args.length
});
```
→ Điều này cho phép filter cho failed requests và xem complete error context trong AWS X-Ray > Trace Details.

### 🔍 Cách xem trong X-Ray Console
Truy cập [CloudWatch Home](https://console.aws.amazon.com/cloudwatch/home) → Application Signals (APM) →Traces
{{< figure src="../../images/3-xray-sdk/001-CloudWatch.png" title="Giao diện CloudWatch Traces" >}}

Filter traces sử dụng annotations, ví dụ:

```
annotation.status = "success"
```
{{< figure src="../../images/3-xray-sdk/002-CloudWatch.png" title="Lọc traces theo annotation trong CloudWatch" >}}

{{< figure src="../../images/3-xray-sdk/003-CloudWatch.png" title="Chi tiết trace trong CloudWatch" >}}
Click vào trace → View subsegments → Check tab **Metadata** hoặc **Annotations**
{{< figure src="../../images/3-xray-sdk/004-CloudWatch.png" title="Annotations và Metadata trong trace CloudWatch" >}}
💡 **Lưu ý:** Structured tracing này làm cho debugging, auditing, và performance monitoring dễ dàng hơn đáng kể trong production environments.