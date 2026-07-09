# Project Heimdall: User Manual

This manual provides operational instructions for managing and interacting with the Project Heimdall automated SOC pipeline.

## 1. System Overview
Project Heimdall is an AI-driven SIEM orchestrator designed to automate Tier 1 SOC triage and eliminate alert fatigue[cite: 1]. The pipeline integrates Wazuh for detection, n8n for orchestration, VirusTotal for threat intelligence, DeepSeek AI for analysis, and Slack for notification[cite: 1]. 

## 2. Prerequisites & Environment Setup
Before operating the system, ensure the following infrastructure and authentication requirements are met:
*   **Host Configuration:** Verify that the host machine virtual memory map count is configured to support the OpenSearch indexer[cite: 1].
*   **Network:** The Wazuh Indexer, Manager, Dashboard, and n8n orchestrator containers must be healthy and running on a shared internal Docker bridge network[cite: 1].
*   **Environment Variables:** Maintain a secure `.env` file containing the DeepSeek API credentials[cite: 1]. 
*   **Authentication:** Ensure you have an active VirusTotal v3 API key and a Slack bot access token[cite: 1]. 
*   **Slack OAuth Scopes:** The Slack bot requires the following scopes to function: `channels:read`, `groups:read`, `chat:write`, and `chat:write.public`[cite: 1].

## 3. Core Operations
The automated pipeline is unidirectional and operates in real time[cite: 1].

*   **Alert Ingestion:** Wazuh monitors endpoints and is configured to forward security alerts of level seven or higher[cite: 1]. These alerts are pushed via a native shuffle integration directly to the internal n8n webhook[cite: 1].
*   **Noise Filtering:** An n8n switch node evaluates the incoming payload using a dynamic JSON path and automatically drops any alerts with a severity lower than seven[cite: 1].
*   **Threat Enrichment:** The system routes the alert to evaluate IP addresses and file hashes, querying VirusTotal v3 API endpoints for network and file indicators[cite: 1]. 
*   **AI Triage:** DeepSeek AI ingests the raw Wazuh payload alongside VirusTotal statistics[cite: 1]. It generates a structured JSON response featuring threat summaries, MITRE framework mappings, and dynamic severity escalation based on malicious hits[cite: 1].

## 4. Interacting with Slack Notifications
When a threat passes the automated triage, n8n pushes a structured Block Kit card to the `security-alerts` Slack channel[cite: 1].

*   **Reviewing Threats:** The Slack notification renders a structured JSON output that includes an enriched VirusTotal threat intelligence section[cite: 1].
*   **Taking Action (Buttons):** You can interact directly with the Slack alert using the embedded buttons. A secondary n8n workflow processes these interactivity webhooks[cite: 1].
    *   **Acknowledge / False Positive:** Clicking these buttons triggers the interactivity workflow, which uses the Slack chat update API to immediately modify the original message, showing the alert as acknowledged or closed[cite: 1]. 
    *   **View in Wazuh:** Clicking this button opens the specific alert directly in your local Wazuh dashboard using the native alerts path[cite: 1].

## 5. Troubleshooting & Maintenance
*   **Wazuh Thresholds:** If you update the minimum severity threshold in the local Wazuh files, you must execute a service restart for the manager to respect the new configuration[cite: 1].
*   **Slack Timeout Errors:** If Slack buttons fail to update the message, verify your ngrok tunnel is active[cite: 1]. The n8n interactivity webhook node must be set to respond immediately to bypass the standard Slack three-second timeout[cite: 1].
*   **DeepSeek Connection Failures:** Do not use the standard n8n OpenAI node wrapper, as it produces a 404 Not Found error due to an endpoint formatting incompatibility[cite: 1]. Ensure the pipeline continues to use standard HTTP Request nodes as a bypass, and verify that the API base URL is clean of any markdown formatting[cite: 1].
*   **VirusTotal API Errors:** If IP address intelligence fails, verify that the VirusTotal IP check node is utilizing the specific IP address API endpoint, rather than the file endpoint[cite: 1].