# Daily AI News Agent Workflow for n8n

An automated n8n workflow that triggers daily at 7:00 AM to harvest, merge, and summarize the latest advancements in AI from multiple external APIs before delivering a highly readable brief to Telegram.

## 🚀 Workflow Overview

This workflow automates the entire process of keeping up with artificial intelligence news:
1. **Trigger:** Fires automatically every day at 07:00 AM.
2. **Data Ingestion:** Simultaneously queries `gnews.io` and `newsapi.org` for English articles matching the query "AI".
3. **Normalization & Merging:** Extracts and passes the arrays through specific Set (`Article`/`Article1`) and Merge nodes.
4. **AI Processing:** Leverages LangChain and an OpenAI model (`gpt-4o-mini`) to dynamically choose the top 15 most relevant stories, synthesize them into 1–2 sentence bullet points, attach their respective source URLs, and enforce strict character constraints (under 2,000 characters).
5. **Delivery:** Dispatches the final formatted digest directly to a configured Telegram chat or channel.

## 🛠️ Architecture & Nodes Used

*   **Schedule Trigger:** Crontab scheduler set to run daily at 7:00 AM.
*   **HTTP Request Nodes:** Pulls from `https://gnews.io/api/v4/search` and `https://newsapi.org/v2/everything`.
*   **Set Nodes (`Article` & `Article1`):** Safely parses individual arrays out of incoming API payloads.
*   **Merge Node:** Aggregates both data flows into a unified branch.
*   **AI Agent (LangChain):** Curates and processes content natively using the connected **OpenAI Chat Model (`gpt-4o-mini`)**.
*   **Telegram Node:** Directly ships the finalized Markdown string down to the user.

## 📋 Prerequisites

To run this workflow without modifications, ensure you have credentials configured for:
*   **n8n instance** (Self-hosted or n8n Cloud, version supporting LangChain integrations)
*   **GNews API Key**
*   **NewsAPI Key**
*   **OpenAI API Key** (or use your n8n free trial credits)
*   **Telegram Bot Token & Target Chat ID**

## 🔧 How to Install

1. **Copy the JSON:** Copy the complete contents of your exported workflow `.json` file from this repository.
2. **Import into n8n:** Open your n8n dashboard, create a new workflow, click the top-right menu icon, and click **Import from File** (or simply paste the JSON code directly onto your empty workspace canvas).
3. **Configure Your Credentials:**
    * Click the **Get Gnews Articles** node and ensure your `apikey` parameter is set.
    * Click the **Get newsApi Articles** node and provide your unique `apiKey`.
    * Link your OpenAI credentials to the **OpenAI Chat Model** node.
    * Enter your custom `chatId` and validate your connection inside the **Send a text message (Telegram)** node.
4. **Deploy:** Turn the workflow toggle from **Inactive** to **Active**.

## 🤖 AI Prompt Tuning

The embedded AI Agent is pre-configured with a hyper-specific instruction system to keep output high-quality and concise:
*   **Filtering:** Looks exclusively for AI technological advancements, tools, research, and applications.
*   **Length Enforcement:** Constrains summaries strictly to 1–2 sentences per topic and caps the entire payload at 2,000 characters to prevent Telegram message fragmentation errors.
*   **Formatting:** Begins each payload using an automated system date expression (`yyyy/MM/dd`) and enforces clear text output without bloated systemic explanations.
