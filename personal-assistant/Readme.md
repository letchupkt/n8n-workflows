<img width="1193" height="769" alt="image" src="https://github.com/user-attachments/assets/ecfdd281-d3b8-42d8-ad2a-3bf08d352d56" />


---

## 🤖 n8n Personal Assistant Workflow

This n8n workflow turns your Telegram chat into a **smart personal AI assistant** powered by **Google Gemini**, **LangChain tools**, and deep integrations with **Google Workspace apps** like Calendar, Gmail, and Sheets.

---

### 🧩 **Features & Integrations**

| Feature                                               | Description                                                                                                |
| ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| 💬 **Telegram Integration**                           | Interact with your personal AI assistant directly from Telegram — chat, give commands, or request data.    |
| 🧠 **Google Gemini Model**                            | Uses `Gemini 2.0 Flash` for natural language understanding and context-aware responses.                    |
| 🧰 **LangChain Agent**                                | Handles reasoning, tool selection, and multi-step actions automatically.                                   |
| 🧍‍♂️ **Simple Memory Buffer**                        | Maintains short-term memory for coherent multi-turn conversations.                                         |
| 🗓️ **Google Calendar Tools**                         | Create, find, or update calendar events directly through AI commands.                                      |
| 📧 **Gmail Integration**                              | Search emails or draft new ones via Gemini — all securely through your Google Service Account.             |
| 📋 **Google Sheets (CRM)**                            | Add, update, or find rows in a contact database stored in Sheets. Perfect for lightweight CRM workflows.   |
| 🌐 **MCP (Model Context Protocol)**                   | Enables communication between multiple AI-enabled workflows or external tools via MCP server/client nodes. |
| 📊 **PostgreSQL or Google Sheets Logging (optional)** | Extend the workflow to log message history, token usage, or AI cost tracking.                              |

---

### ⚙️ **Core Nodes Overview**

* **🧠 Google Gemini Chat Model** – Primary LLM for generating responses
* **🧩 Personal Assistant (LangChain Agent)** – Connects the model, tools, and memory
* **💾 Simple Memory** – Keeps short-term chat context
* **🗓️ Google Calendar Nodes** – Create, update, and list events
* **📧 Gmail Nodes** – Draft and fetch emails
* **📑 Google Sheets Nodes** – CRUD operations on contact data
* **🤝 MCP Server & Client** – Manages AI communication between n8n and external agents
* **💬 Telegram Trigger + Sender** – Handles real-time chat input and output

---

### 🚀 **How It Works**

1. You send a message to your bot on **Telegram**.
2. The **LangChain Agent** processes your request with **Gemini 2.0**.
3. Based on the context, it uses one or more tools (Calendar, Gmail, Sheets, etc.).
4. The result is sent back to Telegram or stored in Google services.

Example commands you can send:

* “Schedule a meeting with John tomorrow at 10 AM.”
* “Find the last 3 emails from Sarah.”
* “Add a new contact: Alex, +1 234 567 890, [alex@example.com](mailto:alex@example.com).”
* “Show my next 5 meetings.”

---

### 🧑‍💻 **Built By**

**Letchu (Lakshmikanthan)**
B.Tech AI & DS | Cybersecurity & Automation Enthusiast
🌐 [Portfolio](https://letchupkt.vgrow.tech) • 💻 [GitHub](https://github.com/letchupkt)  • 💻 [InstaGram](https://instagram.com/letchu_pkt) • 💻 [LinkedIn](https://linkedin.com/in/lakshmikanthank)

---

