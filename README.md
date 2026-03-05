# 🛡️ Heimdall : AI-Driven Security Orchestration

## Overview
SOCopilot is an automated Security Operations Center (SOC) pipeline designed to combat **alert fatigue**. By integrating open-source SIEM capabilities with low-code orchestration and Large Language Models (LLMs), this project demonstrates a modern approach to network defense and threat triage.

Instead of human analysts manually reviewing hundreds of raw logs, SOCopilot automatically intercepts security events, translates them into plain English using AI, assesses the severity, and pushes actionable intelligence directly to a team communication channel.

## 🎯 The Problem It Solves
Modern networks generate massive amounts of log data, leading to alert fatigue where critical threats can be buried under false positives. This project serves as a proof-of-concept for a **SOAR (Security Orchestration, Automation, and Response)** platform that handles Tier 1 alert triage automatically.

## ⚙️ Core Architecture
* **Detection (Wazuh):** Acts as the SIEM, monitoring endpoints for unauthorized access, malware, or anomalies (e.g., SSH brute-forcing).
* **Orchestration (n8n):** The central nervous system that catches Wazuh webhooks and dictates the flow of data.
* **Analysis (OpenAI / Ollama):** Analyzes raw JSON logs, maps them to potential MITRE ATT&CK techniques, and generates human-readable summaries.
* **Notification (Slack):** Delivers formatted, prioritized alert cards to the security team.# 🛡️ SOCopilot: AI-Driven Security Orchestration

## Overview
SOCopilot is an automated Security Operations Center (SOC) pipeline designed to combat **alert fatigue**. By integrating open-source SIEM capabilities with low-code orchestration and Large Language Models (LLMs), this project demonstrates a modern approach to network defense and threat triage.

Instead of human analysts manually reviewing hundreds of raw logs, SOCopilot automatically intercepts security events, translates them into plain English using AI, assesses the severity, and pushes actionable intelligence directly to a team communication channel.

## 🎯 The Problem It Solves
Modern networks generate massive amounts of log data, leading to alert fatigue where critical threats can be buried under false positives. This project serves as a proof-of-concept for a **SOAR (Security Orchestration, Automation, and Response)** platform that handles Tier 1 alert triage automatically.

## ⚙️ Core Architecture
* **Detection (Wazuh):** Acts as the SIEM, monitoring endpoints for unauthorized access, malware, or anomalies (e.g., SSH brute-forcing).
* **Orchestration (n8n):** The central nervous system that catches Wazuh webhooks and dictates the flow of data.
* **Analysis (OpenAI / Ollama):** Analyzes raw JSON logs, maps them to potential MITRE ATT&CK techniques, and generates human-readable summaries.
* **Notification (Slack):** Delivers formatted, prioritized alert cards to the security team.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🛠️ Tech Stack
* **Infrastructure:** Docker & Docker Compose
* **SIEM / XDR:** Wazuh (Manager & Agent)
* **Automation:** n8n
* **AI / LLM:** OpenAI API (or local Llama 3 via Ollama)
* **Alerting:** Slack API / Webhooks

