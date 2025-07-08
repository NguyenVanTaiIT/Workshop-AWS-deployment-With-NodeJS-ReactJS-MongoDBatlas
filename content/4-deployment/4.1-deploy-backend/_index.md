---
title : "Deploy Backend to AWS Elastic Beanstalk"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

In this step, your **Node.js backend** should now be deployed to **AWS Elastic Beanstalk** with:

- AWS X-Ray tracing enabled  
- MongoDB Atlas connected securely via Secrets Manager  
- Environment variables configured properly  
- Health check and graceful shutdown implemented

✅ If you've already completed the setup in [**Section 2.3.1 – Create and Configure Elastic Beanstalk Environment**](../../2-preparation/2.3-create-beanstalk/2.3.1-create-eb-env/), you're good to go!

---

## 🧪 Validate Deployment

Access your Elastic Beanstalk environment (e.g.):

```bash
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```

Then test these endpoints:

```http
GET /api/status
GET /health
GET /api/products
```

You should expect:

- ✅ HTTP 200 responses
- ✅ /health returns xray: enabled and database: connected
- ✅ Traces appear in AWS X-Ray → Service Map

---

## ❗ Didn't deploy yet?

Go back to:
👉 [2.3.1 – Create and Configure Elastic Beanstalk Environment](../../2-preparation/2.3-create-beanstalk/2.3.1-create-eb-env/)

There you'll learn how to:

- Upload your zipped backend code
- Configure environment variables
- Attach IAM roles
- Enable X-Ray daemon

🎉 Once done, you have a full-featured Node.js backend running on Elastic Beanstalk with tracing, observability, and production readiness.
