---
title : "Deploy React Frontend to AWS Elastic Beanstalk"
date: 2025-07-07
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

In this section, you will deploy the **React frontend** of your e-commerce application by combining it into the same deployment as your Node.js backend using **Elastic Beanstalk**.

Unlike S3/CloudFront hosting, this approach uses the Express backend to serve your frontend static files (`dist/`)—allowing seamless routing, cookies, and CORS under the same domain.

---

## 📦 Download Frontend Source Code

[⬇️ Download Frontend Source (frontend.zip)](./../../downloads/ecommerce-frontend.zip)

After downloading:

1. Extract the archive  
2. Inside the extracted folder (`frontend/`), navigate to `src/`  
3. Create a file named `.env.production` with the following content:

```env
VITE_API_URL=https://your-backend-env.ap-southeast-1.elasticbeanstalk.com/api
```
Replace the URL with your actual backend Beanstalk environment.

---

⚙️ **Build the Frontend**
Open a terminal in the frontend/ folder

Run the following commands:

```bash
npm install
npm run build
```
This will generate a `dist`  folder containing production-ready static assets.

---

🚀 **Move dist/ to Backend and Deploy**
Copy the newly generated dist/ folder

Paste it into your `backend/ directory` (next to server.js)

Your backend structure should now look like:

```bash
backend/
├── server.js
├── dist/
│   ├── index.html
│   └── assets/
├── routes/
├── .ebextensions/
└── ...
```

Re-zip the backend folder:

```bash
cd backend
zip -r ../ecommerce-backend-with-frontend.zip .
```

Go to the Elastic Beanstalk Console

- Select your existing ecommerce-app environment



{{< figure src="../../images/4-deployment/001-FrontendDeploy.png" title="Elastic Beanstalk: Upload deployment package" >}}
- Click Upload and deploy
{{< figure src="../../images/4-deployment/002-FrontendDeploy.png" title="Elastic Beanstalk: Select and deploy new version" >}}
- Choose the new zip file: **ecommerce-backend-with-frontend.zip** and label `version-2`
{{< figure src="../../images/4-deployment/003-FrontendDeploy.png" title="Elastic Beanstalk: Version label and deploy" >}}
- Click **Deploy**
---

🌐 **Access the Application**
Visit your Beanstalk environment URL:

```http
http://ecommerce-env.eba-xxxx.ap-southeast-1.elasticbeanstalk.com
```
You should see your React frontend being served — fully integrated with your backend.

{{< figure src="../../images/4-deployment/004-FrontendDeploy.png" title="Frontend deployed and accessible via Beanstalk URL" >}}

---

{{% notice note %}}
The following code in server.js ensures static frontend routing works
{{% /notice %}}


```js
app.use(express.static(path.join(__dirname, 'dist')));

app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'dist', 'index.html'));
});
```

🎉 Your full-stack e-commerce app is now deployed to AWS Elastic Beanstalk — complete with frontend, backend, API, and X-Ray observability.
