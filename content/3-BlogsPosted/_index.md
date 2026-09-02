---
title: "Blogs Posted"
date: 2026-08-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

During the 15-week internship I published **3 in-depth technical blogs** on [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) — analyzing real-world case studies on AWS, covering new service features, cost optimization with IaC, and using AI services to solve concrete enterprise problems.

| #      | Topic                                                                      | Category (per FCAJ requirement)                       | Date       |
| ------ | -------------------------------------------------------------------------- | ----------------------------------------------------- | ---------- |
| Blog 1 | **Automated EC2 & RDS Cost Optimization with AWS Lambda and EventBridge**  | Cost optimization & FinOps automation with Serverless | 01/08/2026 |
| Blog 2 | **Implementing AWS Verified Access in a TIC 3.0 Architecture without VPN** | Advanced Networking & Zero-Trust Access Security      | 15/08/2026 |
| Blog 3 | **Solving automated contract intelligence with Doczy.ai™ on AWS**          | New features: Textract + Bedrock + Smart Chunking     | 07/06/2026 |

---

### [3.1. Blog 1](3.1-Blog1/)

Running non-production environments (Dev, Test, Staging) 24/7 generates massive idle cloud spend for enterprises. This blog introduces an automated FinOps pattern combining **AWS Lambda**, **Amazon EventBridge**, and **Resource Tagging** to dynamically shut down idle EC2 instances and RDS databases outside working hours. By scheduling automated start/stop actions and validating instance tags, this solution helps DevOps teams reduce non-production compute costs by up to **60-70%** without impacting developer workflows.

---

### [3.2. Blog 2](3.2-Blog2/)

Implementing Zero-Trust security for government agencies and enterprises following TIC 3.0 (Trusted Internet Connections) guidelines often faces bottlenecks due to reliance on traditional VPNs. This blog analyzes how to leverage **AWS Verified Access (AVA)** alongside **AWS Network Firewall** and **Amazon Route 53 Resolver DNS Firewall** to build a secure VPN-less access architecture. The solution provides continuous verification based on user identity and device posture, satisfying strict TIC 3.0 compliance controls while optimizing remote worker productivity.

---

### [3.3. Blog 3](3.3-Blog3/)

AArete built **Doczy.ai™** — a contract-intelligence system running on AWS that processed **2.5 million contracts (~50 million pages)** in 22 months, reaching **99% accuracy** (vs. 55% for older rules-based systems), making **137 million Bedrock API calls** and saving customers **~$330M**. This blog walks through the **Textract + Bedrock + Smart Chunking** architecture (AArete's core patent) and **dual clustering** (semantic + structural) — a powerful pattern for legal documents that any team building a RAG system can learn from.
