# Project Heimdall

Project Heimdall is an AI-driven SIEM orchestrator designed to automate Tier 1 SOC triage and eliminate alert fatigue[cite: 1]. The updated pipeline integrates Wazuh, n8n, VirusTotal, DeepSeek AI, and Slack to evaluate and enrich endpoint security events in real time.

## Core Architecture

* **Detection:** Detection utilizes Wazuh to monitor endpoints and push suspicious event data via webhook. Wazuh forwards alerts of level seven or higher using its native shuffle integration directly to the internal n8n webhook.
* **Orchestration:** Orchestration utilizes n8n to filter noise, route data, and query the VirusTotal v3 API for IP address and file hash intelligence.
* **Intelligence:** Intelligence utilizes DeepSeek AI to analyze the raw Wazuh payload alongside VirusTotal metrics, generating a structured JSON response with threat summaries, MITRE mappings, and dynamic severity escalation. DeepSeek correctly ingests VirusTotal statistics and elevates alert severity dynamically based on malicious hits.
* **Notification:** Notification utilizes n8n to parse the AI output and push a structured Block Kit card to a Slack channel for immediate human review.

## Features

* **Containerized Infrastructure:** Fully deployed and stable on a shared internal Docker network. Runs the official Wazuh single-node Docker stack to enforce TLS encryption alongside the n8n orchestrator.
* **Dynamic Noise Filtering:** A Switch node correctly evaluates alert severity using a dynamic JSON path and drops any alerts with a severity lower than seven.
* **Threat Intelligence:** Utilizes routing logic to evaluate alerts for IP addresses and file hashes, querying VirusTotal v3 API endpoints for network and file indicators.
* **Interactive Slack Alerts:** Sends fully formatted interactive alert cards to Slack via the chat postMessage API. A secondary workflow processes interactivity webhooks for Acknowledge and False Positive actions. 
* **Direct Dashboard Access:** Includes a "View in Wazuh" button configured to open the specific alert directly in the local Wazuh dashboard using the native alerts path.

## Prerequisites

* Host machine configured with an appropriate virtual memory map count to support the OpenSearch indexer.
* DeepSeek API credentials secured in an environment file.
* VirusTotal v3 API Key.
* Slack bot access token with the following scopes: `channels:read`, `groups:read`, `chat:write`, and `chat:write.public`.
* An ngrok tunnel (or equivalent) linking the Slack application to the n8n webhook.

## Installation & Setup

1. **Deploy Infrastructure:** Deploy the Wazuh single-node stack and n8n onto the shared Docker bridge network via the `docker-compose.yml` file.
2. **Initialize Orchestrator:** Initialize n8n on `localhost:5678`.
3. **Import Workflows:** Import the primary routing workflow and the secondary interactivity workflow JSON files into n8n.
4. **Configure Webhooks:** Add the n8n integration routing block to the Wazuh dashboard to forward alerts.

## Technical Notes & Workarounds

* **DeepSeek Integration:** The standard n8n OpenAI node wrapper produces a 404 Not Found error when connecting to the DeepSeek API due to an endpoint formatting incompatibility. This pipeline uses standard HTTP Request nodes as a bypass.
* **Slack Interactivity:** The interactivity workflow successfully catches button clicks and bypasses the Slack three-second timeout by setting the webhook node to respond immediately. It updates the original Slack message using the chat update API.
* **VirusTotal API:** The VirusTotal IP check node requires the specific IP address API endpoint rather than the file endpoint. 

---
**Author:** Omar Elwahy
