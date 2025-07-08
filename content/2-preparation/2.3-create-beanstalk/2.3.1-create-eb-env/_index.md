---
title : "Create and Configure Elastic Beanstalk Environment"
date: 2025-07-07
weight : 1
chapter : false
pre : " <b> 2.3.1 </b> "
---

In this step, you will create and configure an **Elastic Beanstalk environment** to host your Node.js backend.

You will:

- Upload the zipped backend source
- Set up environment variables
- Attach the IAM instance role
- Enable AWS X-Ray daemon for tracing
- Deploy and validate the backend application

---

## 📦 Step 1: Prepare Backend Source Code

Download the backend source code (already configured for Beanstalk + X-Ray):

[⬇️ Download Workshop backend Source Code(backend.zip)](../../../downloads/ecommerce-backend.zip)

After downloading, extract and re-zip the contents of the `backend/` folder (not the folder itself):

```bash
cd backend
zip -r ../ecommerce-backend.zip .
```

---

## 🔐 Step 2: Prepare Environment Variables
Use this .env template for local development or apply as environment variables in Beanstalk:

```ini
PORT=8080
ALLOWED_ORIGINS=http://your-backend-url.elasticbeanstalk.com,http://localhost:5173

JWT_SECRET=your-jwt-secret-key
REFRESH_TOKEN_SECRET=your-refresh-token-secret-key

SECRET_NAME=your-secret-name
AWS_REGION=ap-southeast-1
NODE_ENV=development

ENABLE_XRAY=true
XRAY_DAEMON_ADDRESS=127.0.0.1:2000

MONGODB_URI=mongodb+srv://your-username:your-password@cluster0.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0

AWS_ACCESS_KEY=your-aws-access-key
AWS_SECRET_KEY=your-aws-secret-key
```

⚠️ Store sensitive keys (e.g., AWS & MongoDB credentials) securely in AWS Secrets Manager.

---

## ☁️ Step 3: Create Elastic Beanstalk Environment
Go to the [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/home)

{{< figure src="../../../images/2-preparation/033-ElasticBeanstalk.png" title="Elastic Beanstalk Console" >}}

- Click **Create Application**
- Fill in:
  - Application name: `ecommerce-app`
- Under Platform:
  - Platform: Node.js
  - Platform Branch: Node.js 20 running on 64bit Amazon Linux 2023
- Under Application code:
  - Choose Upload your code
  - Upload: **ecommerce-backend.zip**

{{< figure src="../../../images/2-preparation/034-ElasticBeanstalk.png" title="Select Platform and Upload Application" >}}
{{< figure src="../../../images/2-preparation/035-ElasticBeanstalk.png" title="Environment Type and Upload ZIP" >}}

---

## ⚙️ Step 4: Configure Service Access

{{< figure src="../../../images/2-preparation/036-ElasticBeanstalk.png" title="Attach IAM Role to EC2 instance" >}}

{{% notice note %}}
This IAM role allows the EC2 instance to access Secrets Manager, S3, and X-Ray
{{% /notice %}}

---

## ⚙️ Step 5: Enable Monitoring and X-Ray
Under Monitoring **enable AWS X-Ray tracing**

{{< figure src="../../../images/2-preparation/037-ElasticBeanstalk.png" title="Enable X-Ray daemon in environment settings" >}}

Click Apply.

After that, review all settings, then **click Create environment** 

---

## ⚙️ Step 6: Set Environment Variables
Once the environment is up and running:

- Go to the Configuration tab
- Click **Edit**

{{< figure src="../../../images/2-preparation/038-ElasticBeanstalk.png" title="Edit software settings to add environment variables" >}}

Copy the following variables:

```ini
PORT=8080
NODE_ENV=production
AWS_XRAY_DAEMON_ADDRESS=127.0.0.1:2000
JWT_SECRET=your-jwt-secret-key
REFRESH_TOKEN_SECRET=your-refresh-token-secret-key
ALLOWED_ORIGINS=http://localhost:5173,https://your-frontend
```

Click **Add environment property**.
{{< figure src="../../../images/2-preparation/039-ElasticBeanstalk.png" title="Set environment variables" >}}

Click Apply to save changes.

{{% notice tip %}}
💡 You may also add variables like SECRET_NAME, AWS_ACCESS_KEY, and AWS_SECRET_KEY if your app doesn't fetch from Secrets Manager yet.
{{% /notice%}}

---

## ✅ Step 7: Validate Deployment
Once deployed, visit your environment URL (e.g.):

```arduino
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```

Test the following endpoints:

```http
GET /api/status
GET /health
GET /api/products
```

### 🧪 Verify:
- ✅ API responds with 200 OK
- ✅ /health returns:
  - database: connected
  - xray: enabled
- ✅ Traces appear in AWS X-Ray > Service Map

🧩 Troubleshooting
- ❌ App crash or white screen?
  → Check Logs → Last 100 lines in EB console
- ❌ MongoDB not connected?
  → Check if secret mongodb/connection exists and IAM role has permission
- ❌ No traces?
  → Make sure .ebextensions/01_xray.config exists and IAM role has AWSXRayFullAccess

🎉 **Congratulations!**
You have successfully deployed a production-ready backend with AWS Elastic Beanstalk, connected to MongoDB Atlas, integrated with AWS X-Ray, and ready for scaling.
