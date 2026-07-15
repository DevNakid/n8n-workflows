# n8n AI Agent: Google Sheets Emailer

An intelligent n8n workflow that uses an AI Agent to look up contact emails from a Google Sheet and automatically send scheduled event emails via a chat interface.

## How It Works

1. **User Input:** You interact with the workflow via the built-in n8n Chat interface (e.g., *"Send an email to John to schedule our event tomorrow"*).
2. **AI Reasoning:** The AI Agent processes the request and recognizes it needs an email address.
3. **Data Retrieval:** The Agent calls the **Google Sheets Tool** to search for "John" and retrieve his email.
4. **Action execution:** The Agent drafts a professional email template and uses the **Gmail/Email Tool** to send it directly to John.
5. **Confirmation:** The Agent messages you back in the chat confirming the email has been sent.

---

## Workflow Architecture

- **Chat Trigger:** The user-facing chat window.
- **AI Agent (Node):** The brain directing the flow.
- **Language Model:** Connects to an LLM provider (e.g., OpenAI GPT-4o or Anthropic Claude 3.5 Sonnet).
- **Google Sheets Tool:** Allows the AI to query your sheet.
- **Email Tool (Gmail/Outlook):** Allows the AI to dispatch emails.

---

## Prerequisites

Before importing this workflow, ensure you have:
1. **n8n Instance:** Installed and running.
2. **Google Sheet:** A spreadsheet with at least these two column headers in the first row:
   - `Name`
   - `Email`
3. **Credentials:** 
   - Google Sheets API credentials connected in n8n.
   - Email provider API (Gmail OAuth2 or SMTP) connected in n8n.
   - LLM Provider API Key (OpenAI, Anthropic, etc.).

---

## Setup & Configuration

### 1. Google Sheet Structure
Ensure your Google Sheet is formatted simply:

| Name | Email |
| :--- | :--- |
| John Doe | john.doe@example.com |
| Jane Smith | jane.smith@example.com |

### 2. AI Agent System Prompt
Configure your **AI Agent** node with the following instructions:

> "You are an assistant that sends scheduled event emails. When the user asks to send an email to a specific name, first use the Google Sheets tool to find that person's email. Once found, draft a professional email about the scheduled event, and use the Email tool to send it. Confirm to the user when it is sent."

### 3. Tool Descriptions
To ensure the AI knows when to use its tools, make sure their descriptions are set exactly as follows:

*   **Google Sheets Tool Description:** `Use this tool to search for a person's email address by their name. It returns a list of names and emails.`
*   **Email Tool Description:** `Use this tool to send the drafted email once you have successfully retrieved the recipient's email address.`

---

## Usage

1. Open the **Chat Trigger** testing window in n8n.
2. Send a natural language prompt:
   - *"Please send an email to Jane to coordinate our meeting next Tuesday at 10 AM."*
3. The Agent will search, draft, send, and reply with confirmation.
