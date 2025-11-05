# 🧩 Databricks Genie Query Automation — Lab 1 Integration (Governance Foundation)

This n8n workflow integrates directly with **Databricks Genie** to automate conversational queries against the `clientcare.hr_data` catalog objects created in **Lab 1: Building the Governance Foundation**.

---

## 🧠 Overview

The flow lets you ask natural-language questions about the HR data you provisioned in Databricks — for example:

> “What is the highest performing department?”
> “Show me the average salary by hire year.”

Under the hood, n8n talks to the **Genie API** to start a conversation, wait for completion, and then fetch the query results.

---

## ⚙️ Flow Architecture

| Step | Node                         | Purpose                                                                                      |
| ---- | ---------------------------- | -------------------------------------------------------------------------------------------- |
| 1    | **Chat Trigger**             | Captures the question typed in the n8n chat interface.                                       |
| 2    | **Get Genie Space**          | Connects to the existing Genie “hr data analyst” space (`01f0b9da32e415fcb8b53777ebafb119`). |
| 3    | **Start Conversation**       | Sends the chat input to Genie, initiating the analysis request.                              |
| 4    | **Wait**                     | Pauses 15 seconds while Genie executes the underlying SQL or analysis.                       |
| 5    | **Get Conversation Message** | Retrieves the message details and checks Genie’s status.                                     |
| 6    | **Get Query Results**        | Once Genie is in `COMPLETED` state, downloads the structured query output.                   |

---

## 🧰 Pre-requisites (from Lab 1)

1. **Create the service principal** `hr_data_analyst`
2. **Add to the Devs group** and connect to **Serverless Compute**
3. **Create HR tables** under `clientcare.hr_data`
4. **Tag and classify data** (PII, Confidential, etc.)
5. **Create a filtered analyst view** → `clientcare.hr_data.data_analyst_view`
6. **Grant permissions** to the Devs group for the view
7. **Implement column masking** for sensitive fields (e.g. SSN)
8. **Deploy helper functions** for table queries

These steps build the governed dataset that Genie accesses through your n8n workflow.

---

## 🔗 Databricks Connection Details

| Setting        | Example                                                         |
| -------------- | --------------------------------------------------------------- |
| Workspace Host | `https://dbc-fc870b1c-dadf.cloud.databricks.com`                |
| Workspace ID   | `2883439424484179`                                              |
| Genie Space ID | `01f0b9da32e415fcb8b53777ebafb119`                              |
| Credential     | Databricks PAT stored in n8n credentials (`Databricks account`) |

---

## 💬 Sample Query Flow

1. In the n8n Chat UI, type:

   ```
   What is the highest performing department?
   ```
2. Genie executes a secure SQL query over `clientcare.hr_data.data_analyst_view`
3. Results are anonymized per masking rules
4. Output returned in n8n’s result pane or used by downstream nodes

---

## 🧩 Key Features

* Seamless integration between **n8n’s chat interface** and **Databricks Genie**
* Supports **governed, masked data** in Unity Catalog
* Reusable for other **AI-assisted analytics** spaces
* Provides a foundation for **AI Governance & Access-Controlled Queries**

---

## 🚀 Next Steps

* Add a `Loop Until Completed` node to poll Genie until the attachment is ready.
* Extend the workflow with a **“Reply to Chat”** node to display the formatted results.
* Replace Genie calls with **Model Serving endpoint invocations** for production reliability.

---

**Author:** Debasmita Mukherjee
