# 🔐 AWS Login Alert System with CloudTrail, EventBridge, and SNS

### [YouTube Demonstration](https://youtu.be/gPJyF8NFo_g?si=6xdbhE_5vFvnW6XZ)


This project shows you how to build a simple alert system that sends you an email **whenever someone logs into your AWS account**.

Whether it's a root account or an IAM user, this setup will notify you in realtime. It's a perfect project if you're learning **cloud security** and want something **practical and beginner-friendly**. You only need 3 AWS services — and everything is free tier eligible.

---

## 🧠 Why I Built This

One of the first things I wanted to monitor in my AWS account was **login activity**.  
Who logged in? When? Was it the root user?

AWS gives us all the tools — we just need to connect them properly.

So I decided to build a simple system that:
- Logs every console login
- Detects when a login happens
- Sends me an email alert instantly

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

> **Insert architecture diagram here**  
> ![Architecture](screenshots/architecture.png)

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
> ![CloudTrail Dashboard](screenshots/cloudtrail-dashboard.png)

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
> ![Trail Config](screenshots/trail-config.png)

---

### 2. Create an SNS Topic

Now we need a way to send the alert — that’s where SNS comes in.

> **Insert screenshot:** SNS dashboard  
> ![SNS Dashboard](screenshots/sns-dashboard.png)

#### Steps:

1. Open **SNS** from the AWS Console  
2. Go to **Topics**, click **Create topic**  
3. Select **Standard** type  
4. Name the topic: `console-login-alerts`  
5. Leave the other settings as they are and click **Create topic**

> **Insert screenshot:** Topic created  
> ![Topic Created](screenshots/topic-created.png)

---

### 3. Subscribe Your Email to the Topic

1. Scroll down to the **Subscriptions** section  
2. Click **Create subscription**  
3. Protocol: **Email**  
4. Enter your email address  
5. Click **Create subscription**  
6. Now go to your inbox and confirm the subscription

> **Insert screenshot:** Subscription created  
> ![Subscription](screenshots/subscription-confirm.png)

---

### 4. Create an EventBridge Rule

This is the logic that watches for login events and triggers the email.

> **Insert screenshot:** EventBridge rule setup  
> ![EventBridge Rule](screenshots/eventbridge-rule.png)

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
