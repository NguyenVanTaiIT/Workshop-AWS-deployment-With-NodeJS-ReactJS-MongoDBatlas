---
title : "Create and Use Secret in AWS Secrets Manager"
date: 2025-07-07
weight : 3
chapter : false
pre : " <b> 2.2.3 </b> "
---

In this section, you will learn how to securely store and retrieve sensitive credentials using **AWS Secrets Manager**.

Instead of hardcoding MongoDB connection strings or AWS keys in `.env` files or source code, you'll use **AWS Secrets Manager** to store secrets and access them securely from your backend application.

---

## 🔐 Step-by-step: Create a secret in Secrets Manager

1. Go to the [Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/)

2. Click **Store a new secret**

{{< figure src="../../../images/2-preparation/040-SecretsManager.png" title="Store a new secret in AWS Secrets Manager" >}}

---

 3. Choose **Other type of secrets**  
This allows you to define custom key-value pairs manually.

---

 4. In the **Plaintext** section, enter the following (or modify as needed):

```json
{
  "MONGODB_URI": "mongodb+srv://<user>:<pass>@cluster.mongodb.net/ecommerce",
  "AWS_ACCESS_KEY": "<your-access-key>",
  "AWS_SECRET_KEY": "<your-secret-key>"
}
```
{{< figure src="../../../images/2-preparation/041-SecretsManager.png" title="Store a new secret in AWS Secrets Manager" >}}

{{% notice warning %}} 
Replace `<user>`, `<password>`, `<your-access-key>`, `<your-secret-key>` and actual credentials with your own.
{{% /notice %}}

 5. Click **Next**

 6. Enter a name for the secret

Example: `mongodb/connection`


✅ This name will be used in your application to retrieve the secret.

You can optionally add a description and tags.

 7. Leave **automatic rotation** disabled (for now), then click **Next**

 8. On the **Review** page, double-check your inputs  
Then click **Store** to create the secret.

You should see a confirmation message and the new secret listed in the Secrets Manager dashboard.

## 🧪 Confirm the Secret

Click on the secret name (`mongodb/connection`) to open it.  
Verify that the key-value pairs were saved correctly.

{{< figure src="../../../images/2-preparation/042-SecretsManager.png" title="View saved secret in Secrets Manager" >}}

---

## 🧑‍💻 How to use the secret in Node.js (runtime access)

To fetch the secret from your backend app:

1. Install AWS SDK v3 if you haven't:

```bash
npm install @aws-sdk/client-secrets-manager
```
2. Use the following code in your backend (e.g. server.js or a config module):

```js
const { SecretsManagerClient, GetSecretValueCommand } = require('@aws-sdk/client-secrets-manager');

const client = new SecretsManagerClient({ region: 'ap-southeast-1' });

async function getMongoURI() {
  const command = new GetSecretValueCommand({ SecretId: 'mongodb/connection' });
  const data = await client.send(command);
  
  const secret = JSON.parse(data.SecretString);
  return secret.MONGODB_URI;
}
```