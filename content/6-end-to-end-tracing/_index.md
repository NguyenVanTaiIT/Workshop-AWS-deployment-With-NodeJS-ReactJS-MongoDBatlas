---
title : "Trace Requests from Frontend to Database"
date: 2025-07-07
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

In this section, you will perform **end-to-end tracing** of user actions from the React frontend, through the backend, down to MongoDB, all visible in **AWS X-Ray**.

### 🧪 Try This

Open your frontend (hosted on S3 or Beanstalk)

Register a new account  
{{< figure src="./../images/6-end-to-end-tracing/001-EndToEndTracing.png" title="Register a new user account" >}}

Log in using your newly created credentials  
{{< figure src="./../images/6-end-to-end-tracing/002-EndToEndTracing.png" title="Login to your account" >}}

Browse to a product  
{{< figure src="./../images/6-end-to-end-tracing/003-EndToEndTracing.png" title="View product details" >}}

Click "Add to cart", then proceed to checkout  
{{< figure src="./../images/6-end-to-end-tracing/004-EndToEndTracing.png" title="Add product to cart and proceed to checkout" >}}

Place an order  
{{< figure src="./../images/6-end-to-end-tracing/005-EndToEndTracing.png" title="Place the order" >}}

{{< figure src="./../images/6-end-to-end-tracing/006-EndToEndTracing.png" title="Trace appears in AWS X-Ray after placing order" >}}

### 🔍 Now check AWS X-Ray:

- Go to CloudWatch Console → Trace Map
- You should see:
  - Frontend traced request → Backend service (`EcommerceApp`)

{{< figure src="./../images/6-end-to-end-tracing/007-EndToEndTracing.png" title="Trace appears in AWS X-Ray after placing order" >}}


{{< figure src="./../images/6-end-to-end-tracing/008-EndToEndTracing.png" title="Trace appears in AWS X-Ray after placing order" >}}

- Backend → MongoDB operation

{{< figure src="./../images/6-end-to-end-tracing/009-EndToEndTracing.png" title="Trace appears in AWS X-Ray after placing order" >}}

- Click any trace to see Segment details.

### ✅ This proves:
- Frontend sends trace headers (`X-Amzn-Trace-Id`)
- Backend captures and logs segments
- Database activity is also recorded
