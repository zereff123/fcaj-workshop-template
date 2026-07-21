---
title: "Frontend Deployment"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

The cheapest, fastest, and most secure way to host a Single Page Application (like ReactJS) is to use the Amazon S3 and Amazon CloudFront CDN combo paired with OAC (Origin Access Control).

With this architecture, the static source code (HTML, CSS, JS) is securely stored in S3 and is only distributed externally via CDN (CloudFront) at blazing fast speeds.

---

### Build ReactJS Code
Before deploying to the Cloud, your Frontend needs to know the address to communicate with the Backend.
1. Open your ReactJS source code. Find the file containing the API URL configuration variable (Usually `.env` or a constant config file).
2. Replace `http://localhost:8000` with the **API Gateway** URL you obtained in the previous Step (Example: `https://abcd12345.execute-api.ap-southeast-1.amazonaws.com`).
3. Open Terminal and run the command `npm run build` (or `yarn build`).
4. As a result, you will get a folder named `dist` (or `build`) containing the optimally compressed static files.

---

### Hosting static website on Amazon S3 (Fully Private)
We will store files in S3 but **absolutely DO NOT enable Public Access** to avoid source code leaks.

1. Access the **S3** service, click *Create bucket*. Name the bucket (Ex: `quiz-app-frontend-dev-993c6d33`).
2. **Block Public Access settings for this bucket:** MAKE SURE the *Block all public access* box is **CHECKED**.
3. Click *Create bucket*.
4. Click on the newly created Bucket, switch to the **Objects** tab, click Upload and upload all files INSIDE your `dist` folder to S3.

![S3 Frontend Bucket](/fcaj-workshop-template/images/s3-upload.png)

---

### Global Distribution with CloudFront & OAC
This is the most critical step to securely expose the website to the Internet with HTTPS.

1. Access the **CloudFront** service, click *Create a CloudFront distribution*.
2. **Origin domain:** Select your S3 bucket from the suggestions list.
3. **Origin access:** 
   - Select **Origin access control settings (recommended)**.
   - Click *Create control setting*, keep the default name and click Create.
   - This action prompts AWS to automatically generate a Policy that allows only this CloudFront distribution to access the S3 bucket.
4. **Viewer protocol policy:** Select *Redirect HTTP to HTTPS*.
5. **Default root object:** Scroll to the very bottom and type `index.html`.
6. Click *Create distribution*.

Immediately after creation, CloudFront will display a blue banner asking you to update the S3 Bucket Policy.
1. Click the **Copy policy** button.
2. Click the **Go to S3 bucket permissions** link right on that banner to jump straight to S3.
3. Scroll down to the **Bucket policy** section, click *Edit*, paste the copied code, and click *Save changes*.

AWS will need 3 - 5 minutes to distribute your code to hundreds of Edge locations globally. When the CloudFront status changes to *Deployed*, copy the URL string in the *Distribution domain name* section (Example: `d12345abcdef.cloudfront.net`) and open it in your browser.

![CloudFront App](/fcaj-workshop-template/images/cloudfront-app.png)
