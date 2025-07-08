---
title: "Deployment Backend and Frontend"
date: 2025-07-07
weight: 4
chapter: true
pre: "<b> 4. </b>"
---

In this chapter, you will deploy both the **backend (Node.js + MongoDB + AWS X-Ray)** and the **frontend (React + Vite)** of the e-commerce application to AWS infrastructure.

We will cover:

### 📌 4.1 Deploy Backend to Elastic Beanstalk

- Deploy your Node.js backend to AWS Elastic Beanstalk
- Use pre-configured code with:
  - AWS X-Ray tracing
  - MongoDB Atlas integration
  - Secrets Manager
  - Auto health checks and logging
- Verify your deployment with `/health` and `/api/status`
- Ensure traces are visible in the AWS X-Ray Console

👉 See: [4.1 Deploy Backend to AWS Elastic Beanstalk](4.1-deploy-backend/)

---

### 📌 4.2 Deploy React Frontend to AWS

- Build your React Vite frontend
- Deploy using:
  - Option A: Amazon S3 + CloudFront (static hosting)
  - Option B: Second Elastic Beanstalk environment (optional)
- Configure the frontend to talk to your backend securely
- Trigger end-to-end traces from frontend → backend → database

👉 See: [7.2 Deploy React Frontend to AWS (S3/CloudFront or Beanstalk)](4.2-deploy-frontend/)

---

Once this chapter is completed, you will have your **entire MERN stack application** fully deployed on AWS with **production-grade observability** using **X-Ray**, **CloudWatch**, and **Secrets Manager**.
