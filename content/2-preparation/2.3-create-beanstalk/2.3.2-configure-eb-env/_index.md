---
title : "Update Elastic Beanstalk Environment Configuration"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 2.3.2 </b> "
---

In this step, you will **review and update configuration settings** for the Elastic Beanstalk environment you created earlier.

This includes:

- Setting up environment variables
- Ensuring IAM role is attached
- Enabling the AWS X-Ray daemon
- Verifying security group access and instance health

---

 🔧 Step 1: Open Environment Settings

1. Go to [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/)
2. Select your environment (e.g. `ecommerce-env`)
3. Click **Configuration**

---

 🧪 Step 2: Edit Software Settings

Click **Edit** in the **Software** section:

Add or update these environment variables:

| Key                       | Value                                                |
|---------------------------|------------------------------------------------------|
| `PORT`                   | `8080`                                               |
| `NODE_ENV`               | `production`                                         |
| `AWS_XRAY_DAEMON_ADDRESS`| `127.0.0.1:2000`                                     |
| `JWT_SECRET`             | *(your custom JWT secret)*                          |
| `REFRESH_TOKEN_SECRET`   | *(your refresh token secret)*                       |
| `ALLOWED_ORIGINS`        | `http://localhost:5173,https://your-frontend.com`    |

✅ Ensure `PORT` matches what your Node.js server listens on (default is `8080`).

---

 🔐 Step 3: Attach IAM Instance Profile

Click **Edit** in the **Security** section:

- Set **EC2 instance profile** to:  
  `EcommerceAppInstanceRole`  
  *(Created earlier with X-Ray and Secrets Manager permissions)*

This ensures your app can:

- Read secrets from AWS Secrets Manager
- Send trace data to AWS X-Ray
- Access S3 if needed

---

 ⚙️ Step 4: Ensure X-Ray Daemon Is Running

Ensure your `ecommerce-backend.zip` contains:

`xray.config` file

This file configures the EC2 instance to install and start the AWS X-Ray daemon on port `2000`.

Sample `.ebextensions/xray.config`:

```yaml
files:
  "/etc/xray-daemon.cfg":
    mode: "000644"
    owner: root
    group: root
    content: |
      {
        "Daemon": {
          "BindAddress": "127.0.0.1:2000",
          "Region": "ap-southeast-1"
        }
      }

services:
  sysvinit:
    xray:
      enabled: true
      ensureRunning: true
      files:
        - /etc/xray-daemon.cfg
```

---

 ✅ Step 5: Redeploy (if needed)
If you've made changes to your environment settings or zip package:

- Go to Elastic Beanstalk > Application versions
- Upload and deploy the updated .zip
- Wait for green health status

🔍 Confirm everything is working:
- Visit /health endpoint → should show xray: enabled
- Go to AWS X-Ray Console > Service Map → should show your backend service
- Check Logs > Request logs in Beanstalk if issues arise
