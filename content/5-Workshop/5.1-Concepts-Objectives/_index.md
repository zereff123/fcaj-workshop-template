---
title : "Concepts & Objectives"
date : 2026-07-12 
weight : 1 
chapter : false
alwaysopen: true
pre : " <b> 5.1. </b> "

---
In this section, we will explore the core concepts that make up the Galaxy Brain system architecture.

### Project Objectives
The project focuses on designing and building a complete online multiple-choice exam system (Quiz App). It ensures smooth data flow from database storage, Backend business processing, to distributing the Frontend interface to end-users at high speed and absolute cost optimization through a Cloud-Native Serverless architecture.

### Basic AWS Concepts
- **Amazon S3 & CloudFront:** The perfect duo for hosting static websites (Frontend) and distributing content at lightning speed globally, replacing traditional web servers.
- **Amazon API Gateway:** A secure communication gateway, receiving HTTP Requests from the Frontend and routing them to the Backend.
- **AWS Lambda:** Serverless computing service. The Backend application (written in FastAPI) will run automatically on Lambda without the need to manage virtual servers or network zones (VPC).
- **Amazon DynamoDB:** A NoSQL database with automatic scaling capabilities, extremely suitable for Serverless architecture to store user data, quizzes, and results.

