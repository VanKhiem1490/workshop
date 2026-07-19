---
title: "Event 2"
date: 2026-06-27
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---
# HARVEST REPORT: OPERATIONS OPTIMIZATION AND INCIDENT RESPONSE WITH DEVOPS AI AGENTS

### I. Event Overview

* **Topic:** Operations Optimization and Incident Response with DevOps AI Agents
* **Event:** FCAJ Community Day
* **Date & Time:** 09:00, June 27, 2026
* **Location:** 26th Floor, Bitexco Financial Tower, District 1, Ho Chi Minh City
* **Speakers:** Ms. Gia Bao & Mr. Nguyen Nguyen
* **Role:** Attendee

---

### II. Objectives & Orientations

The event centered around elevating the efficiency of cloud infrastructure operations:
* **Popularize AI Solutions:** Introducing the DevOps AI Agent model on AWS, designed to support operations teams and SREs in automated incident detection and resolution.
* **Comprehensive Automation:** Targeting the optimization of Mean Time to Detect (MTTD) and Mean Time to Resolution (MTTR) to protect service continuity.
* **Clarify Agent Architecture:** Exploring the 6 core pillars of DevOps Agents and their standardized 4-step diagnostic workflow (Triage, Investigation, Mitigation, Improvement).
* **Promote Proactive Operations:** Shifting operational mindsets from reactive manual troubleshooting to proactive, human-in-the-loop AI-assisted operations.

---

### III. Key Technical Highlights

The speakers analyzed practical infrastructure pain points and how AI agents solve them:
1. **Challenges in Traditional Operations:** Teams struggle with fragmented telemetry (logs, metrics, and traces scattered across CloudWatch, Grafana, etc.) and organizational context loss, which delays root cause investigations during outages.
2. **6 Core Pillars of DevOps AI Agents:**
   * *Context Learning (Agent Space):* Automatically scans and maps resource dependencies using resource tags to compile a dynamic topology map.
   * *Control (Secure Access):* Restricts permissions to least privilege and accesses isolated resources via secure private connections.
   * *Integration & Collaboration:* Integrates seamlessly with chatops (Slack, ServiceNow) and expands execution capabilities via MCP.
   * *Cost-effectiveness:* Minimizes cost through execution-time-based billing ($0.083/second) rather than token-based billing.
3. **Standardized 4-Step Incident Resolution:**
   * *Triage:* Automatically prioritizes incoming telemetry alerts.
   * *Investigation:* Automatically analyzes logs and topologies to perform Root Cause Analysis (RCA).
   * *Mitigation:* Proposes a safety-first mitigation plan for operator approval before execution.
   * *Improvement:* Generates infrastructure optimization recommendations based on incident history to prevent recurrence.
4. **Live Demonstrations (DDoS Attack Simulation):**
   * *Scenario:* An e-commerce system running on ECS behind an Application Load Balancer (ALB) experiences a DDoS attack of 1,000 requests/second, spiking latency to 12 seconds.
   * *Agent Action:* The DevOps Agent parallelizes the investigation across 5 logical flows. Within 15 minutes, it diagnoses traffic saturation at the ALB and generates a 5-step mitigation plan.
   * *Resolution:* The engineer approves the plan, and the Agent stops the 10 malicious ECS tasks, restoring service normality in seconds.
5. **Prerequisites for Success:** Requires a mature observability foundation (rich logs and metrics) and provides maximum value in complex, large-scale microservice architectures.

---

### IV. Lessons Learned & Practical Applications

Key takeaways from the session include:
* **Development Mindset:** AI is a skills magnifier, not a human replacement. Operational systems must be designed with transparency and observability-driven architectures in mind.
* **Infrastructure Knowledge:** Mastered the integration sequence starting from monitoring triggers, continuing to DevOps Agent inference, and concluding with human-in-the-loop mitigation execution.
* **Action Plan:**
  * Review and standardize logging and alarm configurations on current projects to ensure high-quality telemetry data.
  * Utilize the AWS trial program to deploy a DevOps Agent on a staging environment to measure actual MTTR improvements.

---

### V. Event Experience & Insights

* **Speakers Insights:** The speakers presented compelling, real-world case studies (such as WGU reducing MTTR by 77%), proving the practical feasibility of deploying DevOps Agents in production.
* **Technical Experience:** Witnessing the Agent automatically map over 300 resource dependencies in minutes highlighted the power of AI in simplifying complex AWS environments.
* **New Technology Applications:** Inspired a combined workflow using DevOps Agents (infrastructure management) alongside coding assistants (Amazon Q) to create a self-healing loop: detect issue -> write fix code -> execute patch.

> **Overall Summary:** Integrating DevOps AI Agents is defining the new standard of cloud operations. The seminar provided a clear roadmap to building secure, resilient, and self-healing cloud architectures.

![Event Participation Image](/images/4-EventParticipated/tang36.jpg)
