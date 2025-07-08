---
title : "Enable S3 Upload and Image Hosting"
date: 2025-07-07
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

In this section, you'll **verify and test image uploading** using **Amazon S3**, integrated directly into your backend API.

Your backend has been pre-configured with:
- **AWS SDK v3 (`@aws-sdk/client-s3`)**
- **Multer** for processing form-data uploads
- **X-Ray tracing** for upload activity
- Logic to generate and return **public URLs** from your S3 bucket

---

## ✅ What You'll Do

- Confirm your S3 bucket (e.g., `ecommerce-products-2025`) was created correctly
- Upload product images from the **admin dashboard**
- Verify image URLs are accessible from **Amazon S3**
- Confirm tracing appears in **AWS X-Ray**

---

## 🔍 How the Upload Works

The backend route responsible for file uploads is:

```js
POST /api/products/upload
```
It uses the uploadProductImage controller in controllers/productController.js.

This function does the following:

Parses the file using multer:

```js
const upload = multer();
```
Accepts multipart file as image field

Sends it as a buffer to S3

Initializes the S3 client in your AWS region, using environment variables:

```js
const s3Client = new S3Client({
  region: process.env.AWS_REGION || 'ap-southeast-1',
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY,
    secretAccessKey: process.env.AWS_SECRET_KEY
  }
});
```
Builds a file path and uploads the file:

```js
const params = {
  Bucket: 'ecommerce-products-2025', // Make sure this matches your bucket name!
  Key: `products/${Date.now()}-${req.file.originalname}`,
  Body: req.file.buffer,
  ContentType: req.file.mimetype
};

await s3Client.send(new PutObjectCommand(params));
```
Returns the S3 URL like:

```bash
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/products/your-image.jpg
```

---

## 🧪 Testing the Upload

1. Log in to your app as Admin
2. Go to Admin Dashboard → Add Product
3. Select an image and click Upload
4. Submit the form
5. After saving, you should see the uploaded image appear next to the product.

The image URL should start with:

```bash
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/...
```
✅ You can click this link and view it publicly.

---

## ⚙️ What Needs to Be Correct

✅ **Bucket Name in productController.js:**
Make sure this line:

```js
Bucket: 'ecommerce-products-2025'
```
matches your actual S3 bucket name (the one you created manually or in lab step [2.5.1 - Create S3 Bucket](../../2.5.1-create-bucket/)).

✅ **IAM Role used by Beanstalk must have this policy:**

- AmazonS3FullAccess

Or a custom policy allowing s3:PutObject, s3:GetObject for your bucket

✅ **CORS Settings in the S3 bucket:**
Make sure you’ve configured the following:

```xml
<CORSRule>
  <AllowedOrigin>*</AllowedOrigin>
  <AllowedMethod>GET</AllowedMethod>
  <AllowedMethod>POST</AllowedMethod>
  <AllowedMethod>PUT</AllowedMethod>
  <AllowedHeader>*</AllowedHeader>
</CORSRule>
```

---

## 📦 Sample Environment Variables
These environment variables must be present in your Elastic Beanstalk config or .env:

```env
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY=your-access-key
AWS_SECRET_KEY=your-secret-key
```
Without these, S3Client will not authenticate correctly and uploads will fail.

---

## 🔎 X-Ray Tracing
Every file upload is traced via AWS X-Ray:

**FileValidation segment logs:**
- file size
- mimetype
{{< figure src="./../images/5-s3-upload/001-S3uploads.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
**S3Upload segment logs:**
- image_url
- key
- upload result or error
{{< figure src="./../images/5-s3-upload/002-S3uploads.png" title="Amazon Simple Storage Service (Amazon S3)" >}}
Go to X-Ray Console and navigate to the Service Map to confirm upload activity.

---

🎉 Once verified, you now have a complete S3 image hosting integration with traceable uploads, public file access, and admin-managed product creation.
