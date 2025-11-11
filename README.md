
---

# 🧩 Databricks Custom Agent Query Automation via n8n — Secure HR Analytics

This n8n workflow connects **directly to a custom Databricks Agent endpoint** to automate conversational queries against the governed HR dataset in Databricks — without using Genie.
All queries are executed through Databricks’ **Mosaic AI Agent Framework** and **Unity Catalog controls**, ensuring privacy, masking, and audit logging are always enforced.

---

## 🧠 Overview

Ask questions in plain English — the Databricks Agent converts them into secure, policy-compliant analytics queries.

Examples:

* “Which department shows the highest retention?”
* “What’s the average salary by hire year?”

Every query runs within the Databricks security boundary, using your Unity Catalog governance setup.

---

## ⚙️ Flow Architecture

| Step | Node                                      | Purpose                                                    |
| ---- | ----------------------------------------- | ---------------------------------------------------------- |
| 1    | **Chat Trigger**                          | Captures the user question from the n8n chat interface.    |
| 2    | **HTTP Request – Query Databricks Agent** | Sends the query directly to the Databricks Agent endpoint. |
| 3    | **Respond to Chat**            | Returns the agent’s answer back into n8n’s chat UI.        |

This setup replaces Genie’s conversational flow with a **direct, authenticated model-serving call** to Databricks.

---

## 🧰 Databricks Setup

1. **Deploy the custom agent** (e.g., `hr_analytics_agent`) in Databricks Mosaic AI.
2. **Use Unity Catalog functions** for secure data access (`analyze_performance()`, `analyze_operations()`).
3. **Apply governance controls**: data classification tags, row-level filters, and column masking for PII.
4. **Grant access** only to approved service principals or groups (e.g., `hr_data_analyst`, `Devs`).
5. **Enable MLflow logging** for traceability and compliance review.

These measures ensure all n8n queries respect Databricks’ data-governance policies automatically.

---

## 🔗 Databricks Connection Details

| Setting    | Example                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------ |
| Endpoint   | `https://<your-workspace>.cloud.databricks.com/serving-endpoints/<your-agent>/invocations` |
| Auth Type  | Databricks Personal Access Token (stored securely in n8n)                                  |
| Credential | `Databricks account` (n8n credential entry)                                                |

---

## 💬 Example Query Flow

1. In n8n Chat, type:

   ```
   What is this year's best performing department?
   ```
2. The HTTP Request node calls your Databricks Agent endpoint.
3. The agent executes a governed query over `clientcare.hr_data.data_analyst_view`.
4. Results follow masking and filter policies defined in Unity Catalog.
5. The answer is returned to n8n for display or further automation.

---

## 🧩 Key Features

* 🔒 **Direct integration** with Databricks custom agent (no Genie dependency)
* 🧠 **Governed analytics** via Unity Catalog functions
* 🧾 **Full auditability** through MLflow tracking
* ⚙️ **Reusable architecture** for any Databricks-hosted agent endpoint
* 🌐 **Secure orchestration** using n8n’s credential manager and chat interface

---

**Author:** Debasmita Mukherjee
