---
title: "Backend Deployment"
date: 2026-07-12
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

Once the DynamoDB Database is ready, the next step is to push the Backend source code (FastAPI) to the cloud. We will use AWS's incredibly powerful serverless computing service, **AWS Lambda**, combined with **Amazon API Gateway** to route requests.

Unlike the old architecture that runs Docker 24/7 causing waste, AWS Lambda only charges when an API is called, helping you maximize cost savings.

---

### Creating an IAM Execution Role for Lambda
Before creating the Lambda, we need to create an "identity card" (IAM Role) to grant Lambda permissions to read/write to the DynamoDB table and write logs.

1. Access the **IAM (Identity and Access Management)** service, in the left menu choose **Roles** -> *Create role*.
2. Trusted entity type: Select **AWS service**.
3. Use case: Select **Lambda** then click Next.
4. In the Add permissions step, search for and check these 2 policies:
   - `AWSLambdaBasicExecutionRole` (Permission to write logs to CloudWatch).
   - `AmazonDynamoDBFullAccess` (Permission to interact with DynamoDB. *Note: In a real large project you should create a custom policy granting access only to the ARN copied in Step 1 for absolute security*).
5. Click Next, name the Role `QuizAppLambdaRole`.
6. Click **Create role**.

![IAM Role Permissions](/fcaj-workshop-template/images/iam-role.png)

---

### Deploying Source Code to AWS Lambda
Since our Backend uses Python (FastAPI) and external libraries (like Mangum for Lambda compatibility), you need to prepare a `.zip` file containing all the code and dependencies (the `venv` or `site-packages` directory).

1. Access the **AWS Lambda** service, select **Functions** -> *Create function*.
2. Select **Author from scratch**.
3. Basic information:
   - Function name: `QuizAppBackend`
   - Runtime: Choose **Python 3.13** (Or the version matching your local setup).
   - Architecture: **x86_64**.
4. Permissions:
   - Expand the *Change default execution role* section.
   - Select **Use an existing role**.
   - Choose the `QuizAppLambdaRole` you just created above.
5. Click **Create function**.
6. On the Function details page, scroll down to the **Code source** section, click the **Upload from** button -> choose **.zip file** and upload your zipped code file. Save it.
7. Scroll down to the **Runtime settings** section, click *Edit* and change the **Handler** field to `app.handler` (Since your main code file is named `app.py` and you initialized the Mangum object named `handler`).

![Lambda Code Source](/fcaj-workshop-template/images/lambda-code.png)

---

### Routing with Amazon API Gateway
AWS Lambda doesn't naturally expose itself to the Internet. We need a service to act as a security gate, which is API Gateway.

1. Switch to the **API Gateway** service, scroll down to find **HTTP API** and click *Build*.
2. In the Integrations section, click *Add integration*:
   - Select **Lambda**.
   - AWS Region: Same region as your Lambda.
   - Lambda function: Select `QuizAppBackend`.
3. Set the API name to `QuizAppAPI`. Click Next.
4. Configure routes:
   - Method: Select **ANY**.
   - Resource path: Enter `/{proxy+}` (This configuration catches all requests and forwards them entirely to FastAPI for routing).
   - Integration target: Select your Lambda function.
5. Click Next, leave the default stage as `$default`, and click **Create**.
6. When the API Gateway is successfully created, you will receive an **Invoke URL** string (Example: `https://abcd12345.execute-api.ap-southeast-1.amazonaws.com`). To get this link, look at the left menu and click on **Stages**.
7. **Copy this URL**, this is the "gateway" for the Frontend to call your Backend!

![API Gateway Routes](/fcaj-workshop-template/images/api-gateway.png)
