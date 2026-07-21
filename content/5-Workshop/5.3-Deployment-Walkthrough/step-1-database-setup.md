---
title: "Database Setup"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

The heart of the Galaxy Brain system is the Database that stores questions, quizzes, and user scores. To meet the Serverless and budget-optimization criteria, we will use **Amazon DynamoDB** instead of traditional relational databases.

---

### Provisioning Amazon DynamoDB Tables
Based on the multi-table architecture design of the project, we will need to initialize 4 separate tables. You need to repeat the steps below 4 times to create all 4 tables.

1. Log in to the AWS Management Console, search for **DynamoDB** and select *Create table*.
2. Table details configuration. Create 4 tables with the following key configurations respectively:
   - **Table 1 (Users):**
     - Table name: `QuizApp-Users-dev`
     - Partition key: `user_id` (String)
     - Sort key: *(Leave blank)*
   - **Table 2 (Quizzes list):**
     - Table name: `QuizApp-Quizzes-dev`
     - Partition key: `quiz_id` (String)
     - Sort key: *(Leave blank)*
   - **Table 3 (Questions bank):**
     - Table name: `QuizApp-Questions-dev`
     - Partition key: `quiz_id` (String)
     - Sort key: `question_id` (String)
   - **Table 4 (Exam Results):**
     - Table name: `QuizApp-Results-dev`
     - Partition key: `user_id` (String)
     - Sort key: `result_id` (String)

3. Table settings configuration - Apply to all 4 tables:
   - Select **Customize settings**.
   - Capacity calculator: Select **Provisioned** (This configuration is eligible for the Free Tier).
   - Read capacity and Write capacity: Set both to **5** to ensure it stays within the AWS 25GB free tier limits.
4. Scroll to the bottom and click **Create table**. 
5. The table creation process is incredibly fast. Once all 4 are created, go back to the **Tables** menu to check their *Active* status.


![DynamoDB Tables](/fcaj-workshop-template/images/dynamodb-tables.png)

---

### Retrieving the ARN (Amazon Resource Name)
Once the tables are successfully created, we need to save their identifiers (ARN) so we can later grant the Backend (AWS Lambda) permission to read and write data. For now, you just need to get the ARN of one representative table to know the format.

1. Click on the table name `QuizApp-Users-dev`.
2. In the **Overview** tab, scroll down to the *General information* section.
3. Find the **Amazon Resource Name (ARN)** (It looks like `arn:aws:dynamodb:ap-southeast-1:123456789:table/QuizApp-Users-dev`).
4. **Copy** this ARN prefix to configure in Step 2.

---

### Seed Sample Data
Your DynamoDB tables are currently empty. To have data available for the Frontend to display (quiz questions, user info) right after connecting, we need to run a Python script to automatically inject (seed) sample data into these 4 tables.

1. Make sure you have **Python** and the **Boto3** library (`pip install boto3`) installed on your machine.
2. Ensure you have configured your AWS CLI credentials (run `aws configure` and enter the IAM User's Access Key).
3. Open the Terminal at the root directory of your source code project `e:\quiz_aws`.
4. Run the following command to start seeding data:
   ```bash
   python infrastructure/seed_dynamodb.py --env dev --region ap-southeast-1
   ```
5. Wait for the Terminal to report success `=== Seed hoan thanh! ===`. At this point, you can return to the DynamoDB console, click on **Explore items**, and select the tables you just created to see the data magically appear!
