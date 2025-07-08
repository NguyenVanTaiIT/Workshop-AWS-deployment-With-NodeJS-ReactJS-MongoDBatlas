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
- Module: [`aws-xray-sdk`](https://www.npmjs.com/package/aws-xray-sdk)

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
Sends segments to the local X-Ray daemon at 127.0.0.1:2000.

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

