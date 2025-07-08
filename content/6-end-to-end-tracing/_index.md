---
title : "Trace Requests from Frontend to Database"
date: 2025-07-07
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

In this section, you will perform **end-to-end tracing** of user actions from the React frontend, through the backend, down to MongoDB, all visible in **AWS X-Ray**.

### 🧪 Try This

1. Open your frontend (hosted on S3 or Beanstalk)
2. Browse to a product
3. Click "Add to cart", then proceed to checkout
4. Place an order

### 🔍 Now check AWS X-Ray:

- Go to X-Ray Console → Service Map
- You should see:
  - Frontend traced request → Backend service (`EcommerceApp`)
  - Backend → MongoDB operation
- Click any trace to see detailed breakdown of latency and segments

### ✅ This proves:
- Frontend sends trace headers (`X-Amzn-Trace-Id`)
- Backend captures and logs segments
- Database activity is also recorded
