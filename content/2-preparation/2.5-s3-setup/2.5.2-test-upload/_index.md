---
title: "Test Image Upload to S3"
date: 2025-07-07
weight: 2
chapter: false
pre: " <b> 2.5.2 </b> "
---

After creating your S3 bucket and configuring CORS, it's time to **test image uploads** through the admin interface of your deployed application.

Your backend is pre-configured to accept image files and store them in the correct bucket path using the `uploadProductImage` route.

---

## ✅ What you will verify

- Frontend can upload image files using the admin panel
- Image URLs are stored in S3 under `products/` prefix
- URLs are publicly accessible (based on bucket policy)

---

## 🧪 Step-by-step: Test Upload

1. Go to your **admin dashboard**
   - URL: `http://your-backend-env.elasticbeanstalk.com`
2. Navigate to the **Product Management** or **Add Product** section
3. Fill out required fields like:
   - Product Name
   - Price
   - Category
   - Brand
   - SKU
4. Click **Upload Image** button
5. Select an image file (e.g., `.jpg`, `.png`)
6. Wait for the upload to complete (you should see a preview or file name)
7. Click **Save** or **Create Product**

---

## 🔍 Expected Result

After a successful upload:

- ✅ The image is uploaded to your S3 bucket (e.g., `ecommerce-products-2025`)
- ✅ The product entry will store the image URL like:

```text
https://ecommerce-products-2025.s3.ap-southeast-1.amazonaws.com/products/1699355589999-laptop.jpg
```

✅ You can copy and paste the URL into a browser and see the image

✅ No CORS error in browser console

🧰 Behind the scenes
Your image upload hits this backend endpoint:

```http
POST /api/products/upload
Content-Type: multipart/form-data
Authorization: Bearer <admin-token>
```

This is handled by:

routes/products.js

controllers/productController.js → uploadProductImage()

It uses AWS SDK v3 and X-Ray tracing to log detailed upload behavior.

🧩 Troubleshooting:

|          Problem          |                             Solution                             |
|:-------------------------:|:----------------------------------------------------------------:|
| ❌ Image not visible       | Ensure correct bucket name & region in code                      |
| ❌ Access Denied           | Confirm IAM Role has AmazonS3FullAccess                          |
| ❌ Image URL 403 Forbidden | Make sure bucket policy allows s3:GetObject for the correct path |

🎉 Congratulations!
You’ve successfully completed S3 image upload integration and confirmed that your frontend + backend + bucket configuration works end-to-end.
