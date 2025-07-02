# 🔐 AWS Login Alert System with CloudTrail, EventBridge, and SNS

### [YouTube Demonstration](https://youtu.be/MEgc0qQ3yKE?si=HoNtiARV6MqGkeMB)


This project shows you how to build a simple alert system that sends you an email **whenever someone logs into your AWS account**.

Whether it's a root account or an IAM user, this setup will notify you in realtime. It's a perfect project if you're learning **cloud security** and want something **practical and beginner-friendly**. You only need 3 AWS services, and everything is free tier eligible.

---

## 🗂️ What You’ll Be Using

Here are the 3 AWS services powering this alert system:

- **CloudTrail** - records login events  
- **EventBridge** - detects those events in realtime  
- **SNS (Simple Notification Service)** - sends an email notification  

Everything is native to AWS, and works perfectly together.

---

## 🖼️ Architecture Overview

Here’s a high-level look at how everything connects.

> **Architecture diagram**  
![1](https://github.com/user-attachments/assets/b9065460-c314-4a0e-9fc5-58a001f6751d)



1. A user logs into the AWS Console  
2. CloudTrail captures the login event  
3. EventBridge detects it and triggers a rule  
4. SNS sends an email alert to your inbox

---

## ✅ Let’s Build It – Step by Step

---

### 1. Set Up CloudTrail

CloudTrail is the foundation. It logs all account activity including console logins.

> **Insert screenshot:** CloudTrail dashboard  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

#### Steps:

1. Open **CloudTrail** in your AWS Console  
2. Go to **Trails**, click **Create Trail**  
3. Name it something clear like `login--trail`  
4. Choose **Create new S3 bucket**  
5. You can turn off encryption (SSE-KMS) if not needed  
6. Under Event Type, choose **Management events**  
7. For API activity, select both **Read** and **Write**  
8. Leave everything else as default and click **Create trail**

> **Insert screenshot:** Trail configuration  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

---

### 2. Create an SNS Topic

Now we need a way to send the alert — that’s where SNS comes in.

> **Insert screenshot:** SNS dashboard  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

#### Steps:

1. Open **SNS** from the AWS Console  
2. Go to **Topics**, click **Create topic**  
3. Select **Standard** type  
4. Name the topic: `console-login-alerts`  
5. Leave the other settings as they are and click **Create topic**

> **Insert screenshot:** Topic created  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

---

### 3. Subscribe Your Email to the Topic

1. Scroll down to the **Subscriptions** section  
2. Click **Create subscription**  
3. Protocol: **Email**  
4. Enter your email address  
5. Click **Create subscription**  
6. Now go to your inbox and confirm the subscription

> **Insert screenshot:** Subscription created  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

---

### 4. Create an EventBridge Rule

This is the logic that watches for login events and triggers the email.

> **Insert screenshot:** EventBridge rule setup  
<img src="https://github.com/user-attachments/assets/ddadddab-c164-43a6-a8c3-4a5a0cb46893"/>

#### Steps:

1. Open **EventBridge** in the AWS Console  
2. Go to **Rules**, click **Create rule**  
3. Name it: `login-rule`  
4. For Event source, choose **AWS events**  
5. Select **Custom pattern**, then paste this JSON:

```json
{
  "source": ["aws.signin"],
  "detail-type": ["AWS Console Sign In via CloudTrail"]
}
