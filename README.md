# Minimal LLM Council (AI Decision System)

## Overview
This project implements a "Council of Agents" architecture to evaluate AI responses for safety and clarity. It orchestrates 3 independent agents and 2 distinct judges to produce a structured decision object with confidence scores, risk assessments, and citations.

**Techniques Used:**
* **Multi-Agent Generation:** Cautious, Optimist, and Logic agents.
* **Parallel Judgment:** Separate rubrics for Safety and Clarity.
* **Persistent Logging:** Asynchronous logging to Google Sheets.
* **API Response:** Returns a structured JSON list with strict schema.

## The Architecture
1.  **Input:** Webhook receives a user question.
2.  **Council Tier:** 3 Gemini Agents generate answers in parallel.
3.  **Judicial Tier:**
    * **Safety Judge:** Scans for physical/medical risks (SIDS, choking).
    * **Clarity Judge:** Scans for readability and helpfulness.
4.  **Output:** JSON object containing `Confidence`, `Risks`, and `Citations`.

## Audit Log
The system maintains a persistent log of every decision.
![Audit Log Screenshot](audit_log_screenshot.png)

## Design Decision (Intentionally Not Automated)
**Decision:** I intentionally did not automate the final "blocking" action within the workflow (i.e., the system returns the risk flags but does not silently refuse the answer).

**Why:** Aonxi values "tying scale to proof." Automated safety gating is often a "black box." By returning the raw *Risk Assessment* and *Confidence Score* to the client, we prove that the risk was detected, but we allow the client-side policy (or a human reviewer) to make the final decision on whether to display the content. This ensures transparency and prevents over-censorship while maintaining safety protocols.

## How to Run
1.  Import `LLM_Council_Audit_Workflow.json` into n8n.
2.  Configure Google Gemini and Google Sheets credentials.
3.  Send a POST request to the Webhook URL with `{"question": "..."}`.
