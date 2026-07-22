---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GALAXY BRAIN

## 1. Project Overview

**Galaxy Brain** is an online quiz application developed as part of an academic project. The system focuses on essential business functions and is designed to operate reliably for a limited number of users, primarily lecturers, project evaluators, and members of the testing group.

The application follows a **minimalist** design approach and is deployed on AWS using a fully **serverless architecture**. This architecture reduces infrastructure management requirements, simplifies system operations, and optimizes deployment costs.

### Technology Stack

| Category | Technologies | Purpose |
| :--- | :--- | :--- |
| **Frontend** | React, Vite, Amazon S3, Amazon CloudFront | Build the user interface, store static content, and distribute content with low latency. |
| **Compute** | AWS Lambda, Amazon API Gateway | Deploy the FastAPI backend without managing virtual servers. |
| **Database** | Amazon DynamoDB | Store application data in a serverless NoSQL database with automatic scaling capabilities. |
| **Security** | IAM Roles, AWS Secrets Manager, Origin Access Control (OAC) | Control access based on the principle of least privilege, protect Amazon S3 resources, and securely manage sensitive information. |
| **CI/CD** | GitHub Actions | Automate the application build, testing, and deployment processes. |

---

## 2. Objectives

### General Objective

Develop and complete an academic project through a full software development lifecycle, from building the frontend and backend to deploying the application on the AWS Cloud. The system must operate reliably, provide the required core functions, and support a smooth demonstration before lecturers or the project evaluation committee.

### Specific Objectives

- Successfully deploy the application on AWS using a modern **serverless architecture**.
- Take advantage of eligible AWS Free Tier services, such as AWS Lambda and Amazon DynamoDB, to keep operating costs as low as possible.
- Complete a clear and well-structured deployment walkthrough that allows the system to be reproduced and deployed again.
- Ensure that the application is stable and ready for demonstration in an academic evaluation environment.

---

## 3. Problem Statement and Target Audience

### Problem Statement

When developing academic projects, students often spend a significant amount of time configuring complex AWS infrastructure components, such as Amazon VPC, NAT Gateway, and load balancers. As a result, they may have less time to complete the application's core functionality and may also face a higher risk of infrastructure-related issues during the final demonstration.

Galaxy Brain addresses this challenge through a **serverless cloud-native architecture**. By reducing the dependence on traditional server management, the development team can focus more on business functionality, application quality, and system reliability.

### Target Audience

- Lecturers and academic project evaluation committees.
- Students and members of the application testing group.

---

## 4. System Architecture

The following diagram presents the overall serverless architecture and data flow of the Galaxy Brain application.

![System Architecture](/images/architecture.png)

---

## 5. Implementation Timeline

| Phase | Duration | Main Activities |
| :---: | :---: | :--- |
| **Phase 1** | Weeks 1–3 | Analyze requirements, design the user interface, and initialize Amazon DynamoDB tables. |
| **Phase 2** | Weeks 4–6 | Develop the backend using FastAPI and the frontend using React. |
| **Phase 3** | Weeks 7–9 | Deploy the backend to AWS Lambda and integrate it with Amazon API Gateway. |
| **Phase 4** | Weeks 10–12 | Deploy the frontend to Amazon S3 and Amazon CloudFront, test the system, and complete the final project report. |

---

## 6. Estimated Budget

The following table presents the estimated monthly cost of the AWS infrastructure. The deployment approach is designed for students and prioritizes the use of eligible AWS Free Tier services.

| AWS Service | Configuration | Estimated Monthly Cost |
| :--- | :--- | :---: |
| **AWS Lambda and Amazon API Gateway** | Usage within eligible Free Tier limits | $0.00 |
| **Amazon DynamoDB** | Storage and requests within eligible Free Tier limits | $0.00 |
| **Amazon S3 and Amazon CloudFront** | Static content storage and low bandwidth usage | Approximately $1.00 |
| **Amazon VPC and Networking** | Serverless architecture without a NAT Gateway | $0.00 |
| **Estimated Total** | | **Approximately $1.00 per month** |

> Actual costs may vary depending on traffic volume, data storage, data transfer, the selected AWS Region, and current AWS pricing policies.

---

## 7. Risk Assessment

| Risk | Level | Mitigation Plan |
| :--- | :---: | :--- |
| **Cold Start Latency** | Medium | FastAPI running on AWS Lambda may respond more slowly to the first request after a period of inactivity. The impact can be reduced by optimizing the source code, minimizing the deployment package size, and selecting an appropriate memory configuration (1024 MB). |
| **Increased User Traffic** | Low | AWS Lambda and Amazon DynamoDB can automatically scale based on demand. However, service limits, monitoring, and load testing should still be configured to ensure stable operation. |
| **Unexpected AWS Costs** | Low | Configure AWS Budgets and email alerts when estimated costs reach or exceed $5. Regularly monitor the number of requests, data transfer, and resource usage. |
| **Deployment Failure** | Medium | Use GitHub Actions to standardize the build and deployment process. Maintain environment configurations securely and verify each deployment in a testing environment before the final demonstration. |
| **Unauthorized Access** | Medium | Apply IAM permissions according to the principle of least privilege, protect the Amazon S3 bucket with OAC, and store sensitive values in AWS Secrets Manager. |