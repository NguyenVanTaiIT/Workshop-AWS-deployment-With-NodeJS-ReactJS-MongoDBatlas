---
title : "Instrument Node.js Backend with AWS X-Ray SDK"
date: 2025-07-07
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

In this section, you will explore how **AWS X-Ray SDK** is fully integrated into the Node.js backend to provide tracing for:

- Incoming HTTP requests
- MongoDB queries
- Internal business logic
- Errors and health diagnostics

All necessary code is already implemented in `server.js`. This step is about **understanding, verifying, and testing** the tracing setup.

---

## 📂 Source location

- File: `backend/server.js`
- Module: [aws-xray-sdk](https://www.npmjs.com/package/aws-xray-sdk)

---

## 🧠 Key Features of the Integration

### ✅ 1. Auto-tracing of HTTP requests

```js
const AWSXRay = require('aws-xray-sdk');
AWSXRay.captureHTTPsGlobal(require('http'), true);
AWSXRay.setDaemonAddress('127.0.0.1:2000');
app.use(AWSXRay.express.openSegment('EcommerceApp'));
```
Automatically traces all incoming Express routes.
Captures upstream headers (from frontend or ALB).
Sends segments to the local X-Ray daemon at 127.0.0.1:2000 if run local.

### ✅ 2. MongoDB Tracing via Custom Subsegments

```js
const segment = AWSXRay.getSegment();
const subsegment = segment.addNewSubsegment('MongoDB - Connect');
// ...
await mongoose.connect(uri, options);
subsegment.close();
```
This pattern allows you to trace database-specific logic. It's used in the connectMongoDB() function.

### ✅ 3. CORS Headers & Trace Propagation

```js
allowedHeaders: ['Content-Type', 'Authorization', 'X-Amzn-Trace-Id', 'x-amz-security-token'],
exposedHeaders: ['Set-Cookie', 'X-Amzn-Trace-Id']
```
This setup ensures full trace propagation across frontend → backend → AWS services.

### ✅ 4. Structured Logging with Trace ID

```js
const traceId = req.headers['x-amzn-trace-id'] || 'local';
console.log(`[${traceId}] ${req.method} ${req.originalUrl}`, { headers, body });
```
Allows correlating logs with traces in the X-Ray console.

### ✅ 5. X-Ray Segment Closure

```js
app.use(AWSXRay.express.closeSegment());
```
This is necessary to finalize each trace, and should be used after all routes are registered.

---

### 🩺 Health Check + X-Ray Visibility
The /health endpoint also checks for:

- MongoDB connectivity via ping()
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

## 🧪 Verify in AWS Console
Open X-Ray Console → Service Map

Find EcommerceApp

Inspect traces for:

- Latency
- MongoDB errors
- End-to-end flows from frontend

---

## ✅ Summary
- Your backend is instrumented with X-Ray.
- Traces capture detailed API + DB behavior.
- Logs are correlated with trace IDs.
- Secure and production-ready.

---

## 🎯 Tracing Details with Annotation and Metadata
In this backend project, each major API operation is wrapped using a helper function that adds X-Ray subsegments, with detailed annotations and metadata to enhance observability.

### 📍 Location in Source Code
- File: `controllers/productController.js`
- Wrapper Function: `withXRay(segmentName, fn)`

### 🧩 What Are Annotations and Metadata?
- **Annotations** are indexed key–value pairs — used for filtering in the X-Ray console.
- **Metadata** are key–value data (any JSON type) — used for detailed trace context.

### ✅ Examples in Code
#### getProducts API
When users view the product list, the system captures:

```js
segment.addAnnotation('category', category || 'all');
segment.addAnnotation('page', parseInt(page));
segment.addMetadata('query_params', { category, brand, sort, page, limit });
segment.addMetadata('category_counts', categoryCounts);
```
→ This helps you trace:
- What filters were used
- How many products were found
- Which categories were present

#### uploadProductImage API
When admins upload a product image to S3:

```js
s3Segment.addAnnotation('upload_success', true);
s3Segment.addAnnotation('image_url', imageUrl);
s3Segment.addMetadata('upload_result', {
  imageUrl,
  key: params.Key,
  bucket: params.Bucket
});
```
→ You can inspect uploaded image details or trace failures (e.g., S3 permission issues).

#### createProduct API
During new product creation:

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
→ Lets you trace what product was added and whether the creation succeeded or failed.

### ⚠️ Error Tracing
In case of failure, the following are added:

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
→ This allows filtering for failed requests and viewing complete error context in AWS X-Ray > Trace Details.

### 🔍 How to View These in X-Ray Console
Go to [CloudWatch Home](https://console.aws.amazon.com/cloudwatch/home) → Application Signals (APM) →Traces
{{< figure src="./../images/3-xray-sdk/001-CloudWatch.png" title="CloudWatch Traces" >}}

Filter traces using annotations, e.g.:

```
annotation.status = "success"
```
{{< figure src="./../images/3-xray-sdk/002-CloudWatch.png" title="Filter traces by annotation in CloudWatch" >}}

{{< figure src="./../images/3-xray-sdk/003-CloudWatch.png" title="Trace details in CloudWatch" >}}
Click into a trace → View subsegments → Check **Metadata** or **Annotations** tab
{{< figure src="./../images/3-xray-sdk/004-CloudWatch.png" title="Annotations and Metadata in CloudWatch Trace" >}}
💡 **Note:** This structured tracing makes debugging, auditing, and performance monitoring significantly easier in production environments.

