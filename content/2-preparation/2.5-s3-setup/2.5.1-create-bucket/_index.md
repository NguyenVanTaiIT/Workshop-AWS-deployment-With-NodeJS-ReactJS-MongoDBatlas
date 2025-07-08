---
title: "Create S3 Bucket for Product Images"
date: 2025-07-07
weight: 1
chapter: false
pre: " <b> 2.5.1 </b> "
---

In this step, you'll create a **new Amazon S3 bucket** to store and serve product images for your e-commerce application.

The backend is already preconfigured to upload to a bucket named: `ecommerce-products-2025`


Make sure you create a bucket with **this exact name** (or update the backend code accordingly).

---

## 🪣 Step-by-step: Create a New S3 Bucket

1. Go to the [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home)

2. Click **Create bucket**
{{< figure src="../../../images/2-preparation/044-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
3. Fill in the following:

- **Bucket name**: `ecommerce-products-2025`


- **Region**:  
Choose the same AWS Region where your Beanstalk environment is running (e.g., `ap-southeast-1`)

{{< figure src="../../../images/2-preparation/045-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

---

## 🔐 Step 2: Disable Block Public Access

1. Scroll down to the **Block Public Access settings**

2. Uncheck:  **Block all public access**

3. Acknowledge the warning that pops up, then click **Create bucket**

{{< figure src="../../../images/2-preparation/046-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

{{< figure src="../../../images/2-preparation/047-S3.png" title="Amazon Simple Storage Service (Amazon S3)" >}}

---

## 📛 Step 3: Add Bucket Policy (Optional for Public Read)

If you'd like your product images to be publicly viewable (without signed URLs), add the following **Bucket Policy**:

1. Go to your newly created bucket  
2. Choose the **Permissions** tab  
3. Click **Edit** under **Bucket Policy**, and paste the following:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUserS3Access",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<your-account-id>:user/<your-username>"
      },
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::ecommerce-products-2025/*"
    },
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::ecommerce-products-2025/*"
    }
  ]
}

```
🔍 Explanation of Permissions
|        SID        |                                        Purpose                                       
|:-----------------:|:-------------------------------------------------------------------------------------:|
| AllowUserS3Access | Allows a specific IAM user to upload, read, and delete objects inside the bucket.    
| PublicReadAccess  | Allows the general public (anyone) to read (GET) files in the bucket, such as images. 



👉 Continue: [2.5.2 – Configure CORS for S3 Bucket](../2.5.2-configure-cors/)



