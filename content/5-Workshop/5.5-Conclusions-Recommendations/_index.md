---
title: "Conclusions & Recommendations"
date: 2026-07-12
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

The project has successfully demonstrated deploying an application to AWS using an optimized and cost-effective Serverless approach. Below is the resource clean up guide after the demo.

---

### Resource Clean Up

This is the **MOST IMPORTANT STEP** for any student project using a Cloud platform. After you have presented to the evaluation committee and no longer need to maintain the system, you MUST clean up the created resources.

Although our Serverless system only charges when used, cleaning up to ensure a clean account is a good practice. Here is the safest deletion sequence:

#### 1. Delete API Gateway & AWS Lambda
1. **API Gateway:**
   - Access the **API Gateway** service, select your `QuizAppAPI`.
   - Click the **Delete** button and confirm.
2. **AWS Lambda:**
   - Access **Lambda** -> Functions.
   - Check the `QuizAppBackend` function, click **Actions** -> **Delete**.

#### 2. Delete DynamoDB Tables
1. Access the **DynamoDB** service, select *Tables*.
2. Select each table (Users, Quizzes, Questions, Results) and click **Delete**.
3. *Note:* AWS might ask if you want to create a backup before deleting. Skip this option if you don't need to keep the sample data to avoid storage costs.

#### 3. Delete IAM Role (Optional)
1. Access **IAM** -> Roles.
2. Find the `QuizAppLambdaRole` you created, select it and click **Delete**.

#### 4. Delete Frontend CDN and Storage
1. **CloudFront:**
   - Access CloudFront, check your Distribution, click **Disable**.
   - CloudFront takes about 3-5 minutes to stop distributing. Only after the status changes to *Disabled* will the **Delete** button light up. Click Delete.
2. **Amazon S3:**
   - Access S3. You cannot delete a Bucket if there are still files inside it.
   - Check the Frontend Bucket, click the **Empty** button. Type *permanently delete* to confirm wiping all HTML, JS files.
   - Then, check the Bucket again and click the **Delete** button.

![Billing Dashboard](/fcaj-workshop-template/images/billing.png)


