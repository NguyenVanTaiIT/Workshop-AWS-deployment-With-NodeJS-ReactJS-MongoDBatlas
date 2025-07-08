---
title : "Create IAM Role for Beanstalk"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

Create an IAM role to be attached to Elastic Beanstalk EC2 instances. This allows the instances to interact with AWS X-Ray, S3, and Secrets Manager.

Required policies:
- `AWSXRayFullAccess`
- `AmazonS3FullAccess`
- `SecretsManagerReadWrite`
- `AWSElasticBeanstalkWebTier`

1. Go to the [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles) to view or create roles for your environment.  


2. Click **Create role** at the top of the page.
{{< figure src="../../../images/2-preparation/015-IamRole.png" title="Create a new IAM Role" >}}


3. Under **Trusted entity type**, select **AWS service EC2**  
{{< figure src="../../../images/2-preparation/016-IamRole.png" title="Create a new IAM Role" >}}

4. Click **Next** to move to permissions.

---

## 📌 Attach Policies

5. In the **Permissions** step, search for and select the following policies:

- `AWSElasticBeanstalkMulticontainerDocker`
- `AWSElasticBeanstalkWebTier`
- `AWSElasticBeanstalkWorkerTier`
- `AWSElasticBeanstalkEnhancedHealth`
- `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`
- `SecretsManagerReadWrite` 
- `AWSXRayDaemonWriteAccess` 
{{< figure src="../../../images/2-preparation/017-IamRole.png" title="Create a new IAM Role" >}}
Click **Next**.

---

## 📝 Name and Create the Role

6. Enter a name for the role, for example:`aws-elasticbeanstalk-ec2-role`
7. (Optional) Add a description such as:  
`IAM role for Elastic Beanstalk EC2 instances with access to X-Ray, Secrets Manager, and Beanstalk features.`
{{< figure src="../../../images/2-preparation/018-IamRole.png" title="Create a new IAM Role" >}}
8. Click **Create Role**
9. After creating the role, return to the [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles) and search for the role name you just created  
(e.g., `EcommerceAppInstanceRole`) to verify that it appears in the list.

{{< figure src="../../../images/2-preparation/019-IamRole.png" title="Create a new IAM Role" >}}
10. **Attach Secrets Manager Policy to IAM Role**

To allow your Elastic Beanstalk EC2 instances to securely retrieve MongoDB credentials from AWS Secrets Manager, you need to attach a custom policy to your IAM role (e.g., `aws-elasticbeanstalk-ec2-role`).

**Steps:**

1. Go to the [IAM Roles Console](https://console.aws.amazon.com/iam/home#/roles).
2. Click your role name (e.g., `aws-elasticbeanstalk-ec2-role`).
3. Under the **Permissions** tab, click **Add permissions** → **Create inline policy**.
4. Choose the **JSON** tab, and paste the following:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "arn:aws:secretsmanager:ap-southeast-1:<your-account-id>:secret:mongodb/connection-*"
    }
  ]
}
```

> 🔁 **Replace `<your-account-id>` with your actual AWS account ID.**

5. Click **Next** → Name the policy (e.g., `SecretsManagerAccessPolicy`).
6. Click **Create policy**.

---

### 🔍 What This Policy Does

- **Allows:** `secretsmanager:GetSecretValue`
- **Resource:** Only for secrets named `mongodb/connection-*` in the `ap-southeast-1` region
- **Purpose:** Lets your Node.js app (on Beanstalk) fetch MongoDB connection strings securely at runtime using the AWS SDK.
- **Security:** Access is scoped to only the required secret, avoiding unnecessary exposure.

✅ Once attached, your Beanstalk instance can securely access secrets — no need to hardcode credentials in `.env` files or deploy sensitive data.

✅ Make sure the attached policies look correct.  
You can click the role name to review permissions and trust relationships.

