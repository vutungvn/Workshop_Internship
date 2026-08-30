---
title: "Proposal"
date: 2026-08-24
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Automated Threat Protection

## Automated Threat Protection with AWS WAF & Lambda

---

# 1. Project Summary

**Automated Threat Protection** is a cloud-native security solution designed to automate the detection, response, and mitigation of web application attacks (such as DDoS, Brute Force, Web Scraping, SQL Injection, and Cross-Site Scripting).

The system fully leverages AWS managed services, including **AWS WAF**, **Amazon CloudFront**, **Amazon CloudWatch**, **AWS Lambda**, and **Amazon SNS**. By continuously analyzing logs and metrics in real time, the system proactively identifies suspicious IP addresses, automatically adds them to the **IP Set** blocklist on AWS WAF, and sends instant alerts to the operations team through Amazon SNS.

The solution ensures outstanding scalability, extremely low attack-mitigation latency, optimized operating costs, and minimizes manual human intervention in the security incident response process.

---

# 2. Problem Statement

## Current Problem

Web applications are increasingly becoming primary targets for automated cyberattacks. Traditional defense and incident-response approaches face several limitations:

- **Slow manual response:** During Brute Force or HTTP Flood attacks, operations engineers must manually inspect logs, identify malicious IPs, and add them to the firewall by hand. This process is time-consuming and leads to prolonged service downtime.
- **Lack of real-time automation:** Static firewall solutions cannot flexibly adjust rules based on continuously fluctuating traffic volumes.
- **Wasted system resources:** Attacks that are not blocked at the Edge Location penetrate deep into the backend, overloading application servers and databases.
- **Lack of centralized alerting:** The security team lacks immediate visibility into the scale, frequency, and origin of attack waves.

## Solution

The proposed solution builds an **Automated Threat Protection** system that fully automates the process of detecting and blocking attacking IPs on AWS:

- **Amazon CloudFront** acts as the CDN that distributes content and blocks attacks at the outermost layer (Edge Location).
- **AWS WAF** filters HTTP/HTTPS traffic, applies baseline protection rules, and enforces blocking of IPs in the **IP Set**.
- **Amazon CloudWatch** collects WAF metrics and CloudFront access logs in real time, detecting abnormal traffic thresholds (e.g., a single IP sending more than 100 requests in 5 minutes).
- When an alert is triggered, **Amazon CloudWatch Alarm** invokes an **AWS Lambda Function**. The Lambda function immediately extracts the offending IP and automatically updates the **AWS WAF IP Set** to block access.
- At the same time, **Amazon SNS** sends detailed alerts (via Email/Telegram/Slack) to system administrators for monitoring.

## Benefits

- **Near real-time automated blocking:** Reduces response time from hours to seconds.
- **Edge protection:** Blocks malicious traffic before it reaches backend servers.
- **Cost and resource savings:** Reduces load on internal servers and lowers infrastructure costs during an attack.
- **Serverless operation:** No servers to manage or maintain for the security monitoring system.
- **High scalability:** Easily extend new attack-detection scenarios or integrate additional log sources.

---

# 3. Solution Architecture

The system follows a Cloud-Native Serverless Security architecture on AWS infrastructure.

## Solution Architecture Diagram

![System Architecture](/images/proposal/system_architecture.png)

## AWS Services Used

- Amazon CloudFront
- AWS WAF (Web Application Firewall)
- Amazon CloudWatch (Metrics, Logs, Alarms)
- AWS Lambda
- Amazon SNS (Simple Notification Service)
- AWS IAM (Identity and Access Management)

## Component Design

### Edge & Traffic Filtering Layer

- **Amazon CloudFront:** Distributes the web application, balances load, and optimizes access speed.
- **AWS WAF:** Integrated directly with CloudFront, managing Managed Rule Sets and Custom Rules (including the Blocked IP Set).

### Detection & Monitoring Layer

- **CloudWatch Logs:** Stores and analyzes access logs from AWS WAF and CloudFront.
- **CloudWatch Alarms:** Monitors WAF metrics (e.g., `BlockedRequests`, `AllowedRequests` per IP) and triggers actions when configured thresholds are exceeded.

### Automated Mitigation Layer

- **AWS Lambda:** A serverless compute function (written in Python/Node.js) that contains the processing logic: extracting the attacking IP, calling the AWS WAF API to add the IP to the blocklist, and setting a time-to-live (TTL) for the IP.

### Notification Layer

- **Amazon SNS:** Acts as a pub/sub broker to instantly push incident notifications to developer/security team channels.

### Deployment Workflow

User request

↓

Amazon CloudFront + AWS WAF _(Traffic filtering)_

↓

Amazon CloudWatch _(Log ingestion & threshold checking)_

↓

AWS Lambda _(Automatically extract IP & update WAF IP Set)_

↓

Amazon SNS _(Send incident notification to the Operations Team)_

---

# 4. Technical Implementation

## Implementation Phases

The project is implemented through the following specific phases:

1. Research web attack mechanisms (DDoS, HTTP Flood, Brute Force) and AWS WAF features.
2. Design the overall architecture of the automated attack-response system.
3. Configure Amazon CloudFront as the primary distribution point for the web application.
4. Create an AWS WAF Web ACL, define baseline WAF Rules, and initialize an empty IP Set.
5. Route WAF Logs / Metrics to Amazon CloudWatch.
6. Configure CloudWatch Alarms to detect abnormal traffic fluctuations from individual IPs.
7. Develop the AWS Lambda Function (Python / `boto3`) to implement the WAF IP Set update logic.
8. Set up IAM Policies/Roles ensuring the Principle of Least Privilege for Lambda.
9. Configure the Amazon SNS Topic and Subscribers (Email/Webhook) to receive alerts.
10. Conduct simulated attack testing (Stress Test / HTTP Flood) to verify the system's automation.
11. Optimize detection thresholds and finalize technical documentation.

## Technical Requirements

### Programming Languages & SDKs

- Python 3.x
- AWS SDK for Python (`boto3`) / AWS SDK for JavaScript

### Cloud Infrastructure & Tools

- AWS Management Console
- AWS CLI
- AWS CloudFormation / SAM (Serverless Application Model - _optional deployment method_)

### Testing & Simulation Tools

- Apache JMeter / Locust / `artillery` (Simulating traffic & HTTP Flood attacks)
- Curl / Postman

---

# 5. Implementation Roadmap

The project is implemented across 8 main work phases:

| Phase                                    | Work Content                                                                                                                  |
| :--------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------- |
| **Phase 1 – Project Planning**           | Analyze security requirements; design the automated infrastructure architecture model on AWS.                                 |
| **Phase 2 – Edge Protection**            | Deploy the Amazon CloudFront distribution; create the Web ACL in AWS WAF; initialize the IP Set for the blocklist.            |
| **Phase 3 – Monitoring & Logging**       | Enable WAF Logging and forward logs to CloudWatch Logs; set up CloudWatch Alarms to detect threshold breaches.                |
| **Phase 4 – Automation Logic**           | Write Python source code for the Lambda Function to interact with the AWS WAF API (`GetIPSet`, `UpdateIPSet`); handle IP TTL. |
| **Phase 5 – Alert Integration**          | Configure the Amazon SNS Topic; subscribe to notifications via Email / Slack Webhook / Telegram Bot.                          |
| **Phase 6 – IAM Security & Permissions** | Review and tighten IAM Roles, granting Lambda the minimum required permissions (Least Privilege).                             |
| **Phase 7 – System Testing**             | Execute simulated HTTP Flood / Brute Force attack scenarios; measure response time and fine-tune thresholds.                  |
| **Phase 8 – Project Completion**         | Summarize performance evaluation metrics; finalize the internship report and solution demo documentation.                     |

---

# 6. Budget Estimation

## Infrastructure Cost Estimate

The operating cost of this automated solution is extremely optimized due to the Serverless and Pay-as-you-go model.

| AWS Service              | Cost Description                    | Estimated Cost       |
| :----------------------- | :---------------------------------- | :------------------- |
| **AWS WAF**              | Web ACL & Rule Groups               | ~$5.00/month         |
| **Amazon CloudFront**    | Distribution traffic (Free Tier)    | ~$0.50/month         |
| **Amazon CloudWatch**    | Metrics, Logs & Alarms              | ~$0.30/month         |
| **AWS Lambda**           | Number of requests & execution time | ~$0.05/month         |
| **Amazon SNS**           | Number of alert messages sent       | ~$0.01/month         |
| **Total Estimated Cost** | **Monthly operating cost**          | **~$5.86 USD/month** |

### Cost Optimization Guidance

- **AWS Budgets:** Set up automatic alerts when total resource cost exceeds **$10.00/month**.
- **CloudWatch Log Retention:** Configure a short log retention period (3–7 days) during testing to avoid increased Log Group storage costs.
- **Post-demo Cleanup:** Delete test scenarios, deactivate unused WAF Web ACLs, and remove CloudWatch Alarms and SNS Subscriptions after the internship period ends to avoid unwanted charges.

---

# 7. Risk Assessment

## Risks & Mitigations

- **Risk 1 - Misconfigured thresholds causing False Positives:** A CloudWatch Alarm threshold set too low may result in blocking legitimate users.
  - _Mitigation:_ Set the test threshold to **Count** mode before activating the official **Block** rule; establish a manual override/whitelist process.
- **Risk 2 - IAM permission errors or API Throttling:** The Lambda function may fail to execute due to missing IAM permissions or hitting AWS WAF API rate limits.
  - _Mitigation:_ Apply proper Least Privilege IAM permissions; implement Backoff & Retry techniques in the Lambda source code.
- **Risk 3 - Sudden spike in logging costs:** A very fast-moving attack can drive up CloudWatch Logs storage costs.
  - _Mitigation:_ Limit the scope of collected log data; use WAF's `Sampled Requests` instead of storing full raw logs during testing.
- **Risk 4 - Log processing latency:** Delays in pushing logs from CloudFront/WAF to CloudWatch can slow down the automated response process.
  - _Mitigation:_ Combine WAF Rate-based Rules (processed directly at the WAF engine) in parallel with the Lambda Automation workflow to create a two-layer protection mechanism.

---

# 8. Expected Results

## Technical Results

Upon completion, the project will deliver:

- A web application security solution that automates 100% of the process for blocking abnormal IP addresses.
- A detection and mitigation system with a near real-time response time measured in seconds.
- Full integration of the AWS WAF – CloudFront – CloudWatch – Lambda – SNS service chain, aligned with AWS Best Practices.
- A visual monitoring dashboard on CloudWatch displaying traffic volume, number of blocked requests, and the list of offending IPs.
- An instant incident-alerting system that notifies administrators via SNS.

## Practical Value

This project demonstrates the strong practical applicability of **DevSecOps** thinking and **Serverless Security** architecture in protecting enterprise infrastructure:

- Optimizes operational staffing costs (significantly reducing manual intervention from the SOC/SecOps team).
- Improves the availability and reliability of web applications against threats from the Internet.
- Lays the foundation for expanding into deeper security scenarios, such as integrating AWS Shield and automatically updating malicious IP lists from external Threat Intelligence sources.
