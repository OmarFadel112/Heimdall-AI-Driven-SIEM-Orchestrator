# Project Heimdall: Implementation & Configuration Log

This document outlines the chronological steps and technical configurations executed to build the Project Heimdall pipeline.

## Phase 1: Infrastructure Deployment
* Engineered the containerized infrastructure on a shared internal Docker network[cite: 1]. 
* Configured a secure environment file utilizing DeepSeek API credentials instead of OpenAI[cite: 1]. 
* Deployed the official Wazuh single-node Docker stack to enforce TLS encryption alongside the n8n orchestrator[cite: 1]. 
* Resolved deployment blockers including host virtual memory allocation for OpenSearch, Docker daemon permissions, YAML syntax errors, and n8n directory ownership permissions[cite: 1]. 
* Executed a clean volume reset to recover from a corrupted Wazuh configuration file[cite: 1].

## Phase 2: Orchestration Initialization
* Initialized n8n on localhost port 5678[cite: 1]. 
* Configured credentials for DeepSeek via an OpenAI base URL override and Slack via a bot access token[cite: 1]. 
* Identified the correct Wazuh dashboard menu to safely append the n8n integration routing block, dictating that Wazuh forward alerts of level seven or higher to the internal n8n webhook[cite: 1].

## Phase 3: Pipeline Ingestion & Filtering
* Configured an n8n webhook to ingest Wazuh security alerts[cite: 1]. 
* Set up a switch node to evaluate alert severity using a dynamic JSON path, filtering out noise by dropping alerts with a severity lower than seven[cite: 1]. 
* Tested the noise filter by generating fake SSH brute force and unauthorized sudo attempts on an Ubuntu agent[cite: 1].

## Phase 4: Threat Intelligence Integration
* Built routing logic using a Switch node to accurately identify and evaluate Wazuh alerts for IP addresses and file hashes using an updated payload path[cite: 1]. 
* Configured two VirusTotal HTTP Request nodes to query the v3 API endpoints for network and file indicators[cite: 1]. 
* Identified and resolved a misconfigured URL endpoint in the VirusTotal IP check node, ensuring it used the specific IP address API endpoint rather than the file endpoint[cite: 1].

## Phase 5: AI Analysis Configuration
* Discovered that the standard n8n OpenAI Message a model node produced a 404 Not Found error with the DeepSeek API due to an endpoint formatting incompatibility in the n8n v2 node wrapper[cite: 1]. 
* Implemented standard HTTP Request nodes as a workaround to bypass the buggy OpenAI wrapper node[cite: 1]. 
* Corrected n8n expressions to successfully reference and merge split data from upstream IP and hash checks into the DeepSeek prompt[cite: 1]. 
* Updated the strict JSON system prompt to ingest the Wazuh payload alongside VirusTotal analysis statistics[cite: 1]. 
* Fixed string escaping issues in the DeepSeek HTTP request payload and resolved JSON parsing errors in the DeepSeek AI node[cite: 1]. 
* Addressed workflow execution blockers caused by a 402 Insufficient Balance error on the DeepSeek node by temporarily pinning mock data[cite: 1].

## Phase 6: Slack Notification & Interactivity
* Debugged infinite loading states in the native Slack node by retrieving Docker container logs and identifying missing Slack bot token permissions[cite: 1]. 
* Worked with the Slack engineer to apply the correct OAuth scopes (`channels:read`, `groups:read`, `chat:write`, and `chat:write.public`) and completed a workspace reinstall[cite: 1]. 
* Resolved variable parsing issues by wrapping the Block Kit JSON in double curly brackets[cite: 1]. 
* Replaced native n8n Slack nodes with standard HTTP Request nodes using the chat postMessage API to resolve formatting issues where n8n failed to parse complex Slack Block Kit arrays[cite: 1]. 
* Updated the Slack notification payload to display enriched VirusTotal intelligence and corrected mismatched JSON keys[cite: 1]. 
* Configured a secondary workflow linked via an ngrok tunnel to process Slack interactivity webhooks for Acknowledge and False Positive actions[cite: 1]. 
* Bypassed the Slack three-second timeout by setting the interactivity webhook node to respond immediately[cite: 1]. 
* Updated the original Slack message using the chat update API to reflect the alert status as acknowledged or closed[cite: 1]. 
* Configured a "View in Wazuh" button to open the specific alert directly in the local Wazuh dashboard using the native alerts path[cite: 1].

## Phase 7: Final Validation
* Conducted live testing using a modified Wazuh alert containing a real WannaCry ransomware hash[cite: 1]. 
* Confirmed the DeepSeek AI correctly ingests VirusTotal statistics and elevates alert severity dynamically based on malicious hits and explicit MITRE framework IDs passed in the raw Wazuh logs[cite: 1].