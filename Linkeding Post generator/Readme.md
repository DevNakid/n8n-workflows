## 📌 Overview

Writing consistent, high-performing content on LinkedIn takes significant time and research. This workflow automates the entire production and publishing loop:

1. Takes a raw topic or seed prompt via a form.
2. Performs real-time research using Tavily Search API.
3. Leverages a multi-agent OpenAI pipeline to draft the post, write a scroll-stopping headline, and generate a contextual image prompt.
4. Generates a custom visual with OpenAI (DALL-E).
5. Backs up the generated image into Google Drive.
6. Publishes the post with the generated image directly to LinkedIn.
7. Dispatches an execution confirmation email via Gmail.
[ 📝 Form Submission ]
│
▼
┌────────────────────────────────────────────────────────┐
│                    CREATE CONTENT                      │
│                                                        │
│  [ 🤖 Post Description AI ] ◄──► [ 🌐 Tavily Search ]  │
│          │                                             │
│          ▼                                             │
│  [ 🎨 Image Prompt AI ]                                │
│          │                                             │
│          ▼                                             │
│  [ 🏷️ Title Generator AI ]                             │
└─────────┬──────────────────────────────────────────────┘
│
├──────────────────────────┐
▼                          ▼
[ 🖼️ Generate Image ]      [ 🏷️ Title & Copy Ready ]
│                          │
▼                          │
[ ☁️ Google Drive Upload ]           │
│                          │
└───────────┬──────────────┘
▼
[ 🔗 LinkedIn Publish ]
│
▼
[ ✉️ Gmail Confirmation ]

---

## ✨ Features

- **🌐 Real-Time Web Research**: Employs Tavily via an HTTP tool node so the primary drafting agent gathers up-to-date sources, statistics, and industry context.
- **🧠 Multi-Agent Specialization**:
  - **Copy Agent**: Focuses on narrative flow, readability, and LinkedIn formatting.
  - **Visual Prompt Agent**: Translates the written post into an artistic concept for DALL-E.
  - **Hook / Title Agent**: Generates an attention-grabbing title and headline.
- **🖼️ Automated Creative Generation**: Produces a matching image on demand using OpenAI's image models.
- **📁 Cloud Asset Backup**: Stores every generated image in Google Drive for future reuse.
- **🎯 100% Hands-Free Publishing**: Directly pushes the formatted post and media asset to your target LinkedIn profile or page.
- **📬 Instant Notifications**: Sends a post summary email upon successful execution.

---

## 🔑 Prerequisites & Credentials

Ensure you have active accounts and credentials configured in your n8n instance:

| Service | Node / Integration | Purpose |
| :--- | :--- | :--- |
| **n8n** (Self-hosted or Cloud) | `On form submission` | Captures seed ideas & orchestrates the flow |
| **OpenAI** | `OpenAI Chat Model` & `Generate an image` | Language models (GPT-4o/mini) and DALL-E 3 |
| **Tavily** | `HTTP Request` (Header Auth) | Real-time web search tool for the AI agent |
| **Google Drive** | `Upload file` (OAuth2) | Stores generated media assets |
| **LinkedIn** | `Create a post` (OAuth2) | Publishes content to your feed or company page |
| **Gmail** | `Send a message` (OAuth2) | Sends execution confirmation alerts |

---

## 📦 Setup & Installation

### 1. Import Workflow
1. Download or copy the workflow JSON file (`workflow.json`).
2. In your n8n workspace, navigate to **Workflows** > **Add Workflow**.
3. Select **Import from File** or paste the JSON directly into the canvas.

### 2. Configure Credentials
Link your accounts to the respective nodes:
- Connect your **OpenAI API Key** to all three Chat Model nodes and the Image Generation node.
- Configure Tavily credentials in the **HTTP Request** tool node (see configuration below).
- Authenticate **Google Drive**, **LinkedIn**, and **Gmail** using standard n8n OAuth2 flows.

### 3. Setup Tavily Search Tool
Open the **HTTP Request** node connected under the `Linkedin post Description AI` agent:
- **Method**: `POST`
- **URL**: `https://api.tavily.com/search`
- **Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `Bearer YOUR_TAVILY_API_KEY`
- **Body Parameters**:
  ```json
  {
    "query": "={{ $fromAI('search_query') }}",
    "search_depth": "basic",
    "include_answer": true
  }
