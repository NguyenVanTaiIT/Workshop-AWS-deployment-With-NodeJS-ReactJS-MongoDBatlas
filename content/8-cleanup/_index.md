---
title : "Clean Up AWS Resources"
date: 2025-07-07
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

To avoid **ongoing AWS charges**, it’s important to delete the resources you created during the workshop. This step is especially crucial if you are using the **AWS Free Tier**, as some services (like MongoDB Atlas, EC2, or CloudWatch Logs) may incur charges over time.

---

## 🧹 Resources to Delete

Here’s how to clean up each service:

---

### 1️⃣ Elastic Beanstalk

1. Go to the [Elastic Beanstalk Console](https://console.aws.amazon.com/elasticbeanstalk/)

2. Select your environment (e.g. `ecommerce-app`)

3. Click **Actions** → **Terminate environment**

   ⚠️ This will terminate EC2, Load Balancer, Auto Scaling, and S3 resources created by Beanstalk.
{{< figure src="./../images/8-cleanup/001-CleanUp.png" title="Terminate Elastic Beanstalk environment" >}}

4. Confirm deletion 

{{< figure src="./../images/8-cleanup/002-CleanUp.png" title="Confirm environment termination" >}}



---

### 2️⃣ Amazon S3 Buckets

1. Go to the [S3 Console](https://s3.console.aws.amazon.com/s3/home)
2. Select your bucket (e.g. `ecommerce-products-2025`)

{{< figure src="./../images/8-cleanup/003-CleanUp.png" title="Select S3 bucket to delete" >}}

3. **Empty** the bucket contents first:
   - Click **Empty**

{{< figure src="./../images/8-cleanup/004-CleanUp.png" title="Empty S3 bucket before deletion" >}}

   - Confirm deletion
4. Then click **Delete bucket**

{{< figure src="./../images/8-cleanup/005-CleanUp.png" title="Delete S3 bucket" >}}

{{< figure src="./../images/8-cleanup/006-CleanUp.png" title="Confirm S3 bucket deletion" >}}

{{% notice warning %}}
⚠️ Buckets must be emptied before deletion.
{{% /notice %}}

---

### 3️⃣ MongoDB Atlas Cluster

> If you created a **MongoDB Atlas cluster**, deleting it will stop billing from MongoDB (not AWS).

1. Go to [MongoDB Atlas](https://cloud.mongodb.com)
2. Navigate to your project

3.leave project

{{< figure src="./../images/8-cleanup/007-CleanUp.png" title="Leave MongoDB Atlas project" >}}

{{< figure src="./../images/8-cleanup/008-CleanUp.png" title="Confirm leaving MongoDB Atlas project" >}}

3. Click **Cluster → Terminate**

{{< figure src="./../images/8-cleanup/009-CleanUp.png" title="Terminate MongoDB Atlas cluster" >}}

{{< figure src="./../images/8-cleanup/010-CleanUp.png" title="Confirm cluster termination" >}}

4. Delete the project if not used again

{{< figure src="./../images/8-cleanup/011-CleanUp.png" title="Delete MongoDB Atlas project" >}}

---

### 4️⃣ AWS Secrets Manager

1. Go to [Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
2. Select your secret (e.g. `mongodb/connection`, `ecommerce-secrets`)
3. Click **Actions → Delete**
4. Confirm deletion

{{< figure src="./../images/8-cleanup/012-CleanUp.png" title="Delete secret in AWS Secrets Manager" >}}

Secrets are scheduled for deletion in 7 days (you can force immediate deletion via CLI).

---

### 5️⃣ IAM Users and Roles

1. Go to the [IAM Console](https://console.aws.amazon.com/iam/)
2. Delete:
   - IAM user (e.g. `ecommerce-user`)
   - IAM roles (e.g. `EcommerceAppInstanceRole`)
   - IAM policies created for the workshop (if custom)

✅ This ensures your workshop credentials are not exposed or reused accidentally.

---

### 6️⃣ CloudWatch Logs (Optional)

1. Go to [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/)
2. Navigate to **Logs → Log groups**
3. Select groups such as:
   - `/aws/elasticbeanstalk/...`
   - `/aws/lambda/...` (if used)
4. Click **Actions → Delete log group**

⚠️ CloudWatch logs may incur costs over time if retained.

---

### ✅ Final Tip

- Wait a few minutes after deleting everything.
- Go to the [Billing Console](https://console.aws.amazon.com/billing/home) to check for **active resources or costs**.

If you see unexpected charges:
- Use **AWS Cost Explorer**
- Check **Resource Groups** or **Trusted Advisor**

---

🎉 **You’ve Successfully Cleaned Up!**

Thanks for completing the workshop. You’ve built a full-stack AWS application and now closed all active resources to prevent charges.


