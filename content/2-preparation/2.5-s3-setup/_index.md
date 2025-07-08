---
title: "Setup Amazon S3 for Image Upload"
date: 2025-07-07
weight: 5
chapter: false
pre: " <b> 2.5. </b> "
---

# ☁️ Amazon S3 Overview
{{< figure src="../../../images/2-preparation/043-S3.png" title="Amazon Simple Storage Service (Amazon S3)" width="150">}}
**Amazon S3 (Simple Storage Service)** is a scalable, durable, and highly available object storage service provided by AWS. It allows developers to store and retrieve any amount of data at any time, from anywhere on the web.

### 💡 Common Use Cases

- Hosting static files like HTML, CSS, JS
- Storing user-uploaded images, videos, or documents
- Serving downloadable content
- Acting as backup or archive storage
- Supporting data lakes and machine learning workflows

---

## 🛍️ What S3 Does in This Workshop

In this e-commerce workshop, Amazon S3 is used to:

- Store uploaded **product images**
- Serve images via **public URLs**
- Integrate securely with your backend using **IAM and AWS SDK**
- Log and trace upload activity using **AWS X-Ray**

Your backend is preconfigured to upload product images to a specified S3 bucket (e.g., `ecommerce-products-2025`) and return accessible URLs for display in the UI.

---

## ✅ What You'll Do in This Section

- Create a new S3 bucket
- Set up permissions and **public access**
- Configure **CORS policy** to allow uploads from your frontend
- Upload an image from the admin dashboard
- Verify that the image is accessible online
- Confirm that upload activity is **traceable in AWS X-Ray**

---

## 📁 Related Source Code

This functionality is handled in the backend by:

📦 backend/
└── controllers/productController.js
└── uploadProductImage() // S3 + X-Ray integration

You can test it via:

```http
POST /api/products/upload
```

👉 Continue to the next step: Create S3 Bucket
