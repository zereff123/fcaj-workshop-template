---
title : "Architecture Design"
date : 2024-01-01 
weight : 2 
chapter : false
alwaysopen: true
pre : " <b> 5.2. </b> "
---
In this lab, we will delve into analyzing the Serverless architecture of the Galaxy Brain system.

The system is designed with a minimalist Cloud-Native mindset, completely eliminating the management of network infrastructure (VPC, Subnet) or traditional servers to maximize performance, security, and automatic scaling capabilities.

![AWS Architecture Diagram](/fcaj-workshop-template/images/2-Proposal/architecture.png)
*AWS Architecture Diagram of the Galaxy Brain system*

---


### Core System Components
Our architecture is divided into specialized processing flows:

1. **Frontend Delivery Flow:**
   - Uses **Amazon S3** to store the static files of the ReactJS application (HTML, CSS, JS).
   - Integrates with **Amazon CloudFront (CDN)** for global content distribution. CloudFront uses the OAC (Origin Access Control) security mechanism to block direct access to S3.

2. **API & Auth Flow:**
   - **Amazon API Gateway** acts as the receiving portal for all interactions from Users and Admins.
   - Requests are forwarded to **AWS Lambda** - which contains the entire Backend logic written in a **FastAPI Monolith**.
   - **AWS Secrets Manager** is called to safely decrypt keys (JWT Secret) serving the user Token authentication process.

3. **Data & Storage Flow:**
   - **Amazon DynamoDB** (NoSQL database) stores User, Quiz, Question, and Result information quickly using a Single-Table model.
   - Attached images of questions (Media) are uploaded directly to a separate **S3** Bucket using the Presigned URL mechanism.
   - **IAM Role** is provisioned following the Least Privilege principle, acting as an Execution Role so Lambda has permission to access the DB and S3.

4. **Observability:**
   - All system logs, access logs, and application errors from Lambda are automatically pushed to **Amazon CloudWatch Logs** for easy monitoring and troubleshooting.