## 🤖 Multi-Channel YouTube Shorts Automation Workflow (n8n)

This n8n workflow automates the entire process of creating, generating, and publishing YouTube Shorts for **three distinct YouTube channels** in parallel every day.

The workflow leverages Google Gemini to write the script/metadata and an external Video Generation API (Veo 3, as inferred from the node names/URLs) to produce the video file, followed by uploading the video to YouTube and logging the result to Google Sheets.

The three automated channels are:
1.  **MindRift:** Motivational content.
2.  **CyberPulse:** AI/Tech/Hacking content.
3.  **WealthOrbit:** Financial/Side Hustle content.

***

### ⚙️ Prerequisites

You will need access to and credentials for the following services:

1.  **Google Gemini API:** For content generation (scripts and metadata).
2.  **Video Generation Service (Veo 3):** For creating the actual video from the script.
3.  **Google Sheets:** To log successful video uploads.
4.  **YouTube Data API:** OAuth2 credentials for uploading videos to each of the three channels.
5.  **SMTP Server:** For sending success and error email notifications.

***

### 🔧 Setup Instructions

The workflow requires setting up multiple **Credentials** and updating **Configuration Variables** in three parallel flows.

#### 1. Configure Credentials

You must ensure the following credentials are set up in your n8n instance and linked to the corresponding nodes:

| Service | Credential Type | Nodes Used In | Note |
| :--- | :--- | :--- | :--- |
| **Google Gemini Chat Model (1, 2, 3)** | Google Gemini (PaLM) API | `Google Gemini Chat Model (1, 2, 3)` | Used for content generation. |
| **Video Generation API (Veo 3)** | HTTP Header Auth | `Start Video Generation (Veo 3) (1, 2, 3)`, `Check Video Status (1, 2, 3)` | Authenticates requests to the video generation service. |
| **YouTube Upload (x3)** | YouTube OAuth2 API | `Upload to YouTube`, `Upload to YouTube1`, `Upload to YouTube2` | **Requires separate OAuth2 credentials for *each* of your three YouTube channels.** |
| **Google Sheets** | Google Sheets OAuth2 API | `Log to Google Sheets`, `Log to Google Sheets1`, `Log to Google Sheets2` | Authenticates access to your master log sheet. |
| **Email Notifications** | SMTP | `Success Email Notification (1, 2, 3)`, `Error Email Notification` | Used for sending alerts. |

#### 2. Update Configuration Variables

There are three **Config Variables** nodes. You must update the values in each node with the correct credentials and details for the corresponding channel flow.

| Flow / Node | Variables to Update | Value Example |
| :--- | :--- | :--- |
| **MindRift Flow** (`Config Variables`) | `geminiApiKey` | `YOUR_GEMINI_API_KEY_HERE` |
| | `veo3ApiKey` | `YOUR_VEO3_API_KEY_HERE` |
| | `veo3ProjectId` | `YOUR_VEO3_PROJECT_ID_HERE` |
| | **`sheetsId`** | `YOUR_GOOGLE_SHEETS_ID_HERE` (ID of your upload log sheet) |
| | `notificationEmail` | `your-email@example.com` |
| **CyberPulse Flow** (`Config Variables1`) | `geminiApiKey`, `veo3ApiKey`, `veo3ProjectId` | (Same as above, or channel-specific if needed) |
| | **`sheetsId`** | `YOUR_GOOGLE_SHEETS_ID_HERE` |
| | `notificationEmail` | `your-tech-email@example.com` |
| **WealthOrbit Flow** (`Config Variables2`) | `geminiApiKey`, `veo3ApiKey`, `veo3ProjectId` | (Same as above, or channel-specific if needed) |
| | **`sheetsId`** | `YOUR_GOOGLE_SHEETS_ID_HERE` |
| | `notificationEmail` | `your-finance-email@example.com` |

***

### 🧠 Workflow Operation Overview

The entire workflow is triggered once daily at **9:00 AM IST** by the `Daily Trigger 9AM IST` node. It then executes three parallel automation flows and features a dedicated error handling path.

#### 1. Content Generation (AI Agent & Gemini)

* **Config Variables (x3):** Sets the API keys, channel name, niche, and Google Sheets ID for the specific flow.
* **AI Agent (x3):** Each agent node contains a unique, detailed prompt instructing Gemini to act as a scriptwriter for that channel's niche (Motivational, Tech, or Finance). The prompt strictly requires the output to be **valid JSON** containing the `title`, `hook`, `script`, `tags`, and `hashtags`.
* **Parse & Format Content (x3):** This custom Code node extracts the JSON from the Gemini output, formats a proper YouTube description using the generated script and hashtags, and passes the structured data to the next step.

#### 2. Video Generation and Polling (Veo 3)

* **Start Video Generation (Veo 3) (x3):** An HTTP Request node sends the generated `title` and `script` to the Video Generation API endpoint. Crucially, it sets the video style parameters based on the channel's niche (e.g., MindRift uses `music_style: "epic_inspirational"`).
* **Save Operation ID (x3):** Extracts the long-running operation ID from the API response.
* **Wait 45s (Initial) (x3):** A fixed wait to give the video generation process a starting buffer.
* **Check Video Status (x3) / Is Video Ready? (x3) / Wait 30s (Retry) (x3):** This loop implements **polling**. It repeatedly checks the status of the video generation using the operation ID. If the video is not `done`, it waits 30 seconds and checks again, up to 15 times, allowing up to 7.5 minutes for video generation.

#### 3. Upload and Logging

* **Extract Video URL (x3):** Once the video status is `done`, this node extracts the final download URL.
* **Download Video File (x3):** An HTTP Request node downloads the binary video file, saving it as a file asset in the workflow data.
* **Upload to YouTube (x3):** Uses the YouTube node to upload the video file. It uses the generated **title** and automatically attaches the video file from the previous step.
* **Format for Logging (x3):** A Code node prepares the final data (Title, URL, Upload Timestamp, Status) into the structure required for the Google Sheet.
* **Log to Google Sheets (x3):** Appends the video details to the "Upload Log" sheet in the specified Google Sheet ID.
* **Success Email Notification (x3):** Sends an email notification confirming the successful upload and logging.

#### 4. Error Handling (Workflow Error)

* **On Workflow Error:** This trigger runs whenever any node in the main workflow fails.
* **Format Error Details / Error Email Notification:** Captures the error message and the failed node's name, then sends an email alert to the designated notification email, allowing for prompt investigation.
