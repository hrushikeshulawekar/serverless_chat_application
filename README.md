# serverless_chat_application
# 🚀 Serverless Real-Time Chat Application on AWS

A Serverless Real-Time Chat Application built on AWS that enables users to send and receive messages instantly using WebSocket APIs without managing any servers.

## 📖 Overview

This project demonstrates how to build a scalable, event-driven, and serverless chat application using AWS services. The application leverages API Gateway WebSocket APIs for real-time communication, AWS Lambda for backend processing, and DynamoDB for storing active client connections.

The frontend is hosted on Amazon S3 and distributed globally using Amazon CloudFront for low-latency access.

---

## 🏗️ Architecture

![Architecture Diagram](architecture-diagram.png)

### Architecture Components

* 🔌 Amazon API Gateway (WebSocket API)
* ⚡ AWS Lambda
* 🗄️ Amazon DynamoDB
* 📦 Amazon S3
* 🌍 Amazon CloudFront
* 📜 AWS CloudFormation

---

## ⚙️ Services Used

### 🔌 Amazon API Gateway (WebSocket)

Provides real-time bidirectional communication between connected clients.

### ⚡ AWS Lambda

Handles:

* User Connections (`$connect`)
* User Disconnections (`$disconnect`)
* Message Processing (`sendmessage`)
* Default Route Handling (`$default`)

### 🗄️ Amazon DynamoDB

Stores active WebSocket connection IDs and supports message broadcasting.

### 📦 Amazon S3

Hosts static frontend files such as:

* HTML
* CSS
* JavaScript

### 🌍 Amazon CloudFront

Distributes frontend content globally with low latency.

### 📜 AWS CloudFormation

Automates provisioning of backend resources including:

* Lambda Functions
* DynamoDB Tables
* IAM Roles & Policies

---

## 🔄 Application Workflow

### 1️⃣ User Connects

* Client establishes WebSocket connection.
* API Gateway invokes `$connect`.
* Lambda stores connection ID in DynamoDB.

### 2️⃣ User Sends Message

* Client sends:

```json
{
  "action": "sendmessage",
  "message": "Hello Everyone!"
}
```

* API Gateway routes request to `sendmessage`.
* Lambda retrieves active connection IDs from DynamoDB.
* Message is broadcast to connected users.

### 3️⃣ User Disconnects

* API Gateway invokes `$disconnect`.
* Lambda removes connection ID from DynamoDB.

---

## 📂 Project Structure

```bash
serverless-chat-app/
│
├── CloudFormation/
│   └── template.yaml
│
├── Lambda/
│   ├── ConnectHandler/
│   ├── DisconnectHandler/
│   ├── SendMessageHandler/
│   └── DefaultHandler/
│
├── Frontend/
│   ├── index.html
│   ├── app.js
│   └── styles.css
│
├── Architecture/
│   └── architecture-diagram.png
│
└── README.md
```

---

## 🚀 Deployment Steps

### Prerequisites

* AWS Account
* AWS CLI
* CloudFormation
* Node.js
* wscat

Install wscat:

```bash
npm install -g wscat
```

### Deploy CloudFormation Stack

```bash
aws cloudformation deploy \
--template-file template.yaml \
--stack-name serverless-chat
```

### Create WebSocket API

Configure routes:

```text
$connect
$disconnect
sendmessage
$default
```

Route Selection Expression:

```text
$request.body.action
```

Attach corresponding Lambda integrations and deploy the API.

---

## 🧪 Testing

Connect to WebSocket:

```bash
wscat -c wss://YOUR_API_ID.execute-api.REGION.amazonaws.com/production
```

Send Message:

```json
{
  "action": "sendmessage",
  "message": "Hello Everyone!"
}
```

Expected Output:

```text
Hello Everyone!
```

---

## 🎯 Key Learnings

* AWS Serverless Architecture
* API Gateway WebSocket APIs
* AWS Lambda Event Processing
* DynamoDB Integration
* CloudFormation Deployment
* Real-Time Messaging Systems
* AWS Troubleshooting & Debugging

---

## 🔍 Challenges Faced

One of the major challenges encountered during implementation was WebSocket routing. Messages were unexpectedly being routed to the default handler instead of the `sendmessage` route.

Troubleshooting involved:

* Verifying Route Selection Expression
* Reviewing API Gateway Integrations
* Redeploying API Stages
* Testing with wscat
* Recreating the WebSocket API

This exercise provided valuable hands-on experience in AWS troubleshooting and debugging.

---

## 🚀 Future Enhancements

* Amazon Cognito Authentication
* Chat History Persistence
* Private Messaging
* User Presence Indicators
* File Sharing Support
* CI/CD Pipeline Integration
* Monitoring with CloudWatch
* Security using AWS WAF

---

## ⭐ If you found this project useful

Give it a star ⭐ and feel free to contribute or provide feedback.
