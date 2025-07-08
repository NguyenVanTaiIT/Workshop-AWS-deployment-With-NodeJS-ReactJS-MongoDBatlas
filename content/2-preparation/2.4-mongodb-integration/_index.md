---
title : "Integrate MongoDB Atlas and Mongoose"
date: 2025-07-07
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

In this section, you will verify how the backend application securely connects to **MongoDB Atlas** using **Mongoose** and AWS **Secrets Manager**.

You don't need to write the logic from scratch. The complete integration has already been implemented in the `server.js` file included in the workshop source code.

---

## 📦 Download Source Code

👉 [Download Workshop Code (.zip)](https://your-link.com/aws-xray-workshop-src.zip)

Extract and open the `backend/` folder. All database connection logic is located in `server.js`.

---

## 🧠 How It Works

- AWS Secrets Manager stores the MongoDB connection string under the key `mongodb/connection`
- At runtime, the app retrieves that value using the AWS SDK:

```js
const client = new SecretsManagerClient({ region: 'ap-southeast-1' });
const command = new GetSecretValueCommand({ SecretId: 'mongodb/connection' });
const data = await client.send(command);
const uri = JSON.parse(data.SecretString).MONGODB_URI;
```
Then it connects to MongoDB using:

```js
await mongoose.connect(uri, { ...connectionOptions });
```

---

## 🔍 What to Look For
Open this function in server.js:

```js
const connectMongoDB = async (maxRetries = 5, retryDelay = 3000) => { ... }
```
This includes:

- Auto-retry connection logic
- Timeout settings
- Pool sizing
- Graceful error handling

---

## 🧪 Verify the Connection
Once you deploy the backend:

- Go to /health endpoint
- You should see:

```json
"services": {
  "database": "connected",
  "xray": "enabled"
}
```
If MongoDB is disconnected or credentials are wrong, the status will show `"database": "disconnected"`.

---

## 🛡️ Security Reminder
- MongoDB credentials are not hardcoded
- Secrets are stored in AWS Secrets Manager
- Only the Beanstalk EC2 role can access them

✅ With this setup, your backend is securely and reliably connected to MongoDB Atlas in production-ready fashion.

---
