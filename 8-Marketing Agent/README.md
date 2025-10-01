# Marketing Agent

<img width="1043" height="514" alt="image" src="https://github.com/user-attachments/assets/0ffcd60f-95b7-4726-8736-89644a13df95" />


# n8n Workflow Documentation: Marketing Agent

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Marketing Agent                                                       |
| **Purpose**       | To automate the lead generation, qualification, and initial follow-up process for marketing. The AI Agent processes new leads submitted via a web form and uses various tools (Google Sheets, Gmail, Telegram) to execute sales actions. |
| **Trigger**       | Web Form Submission                                                  |
| **Dependencies**  | Google Gemini Chat Model, Google Sheets, Gmail, Telegram             |
| **Version ID**    | 6af210e7-8898-4ed9-897d-606d2d415170                                |

---

## 2. Node Configuration Summary
| Node Name                                   | Type                  | Description                                                                 |
|---------------------------------------------|-----------------------|-----------------------------------------------------------------------------|
| **On form submission**                      | Form Trigger          | Primary Trigger. A public web form collecting lead information (Company Name, Email, Budget). |
| **Google Gemini Chat Model**                | Chat Model            | Provides the language model (`models/gemini-2.5-pro`) for the AI Agent.     |
| **Simple Memory**                           | Memory Buffer Window  | Stores the submitted form data for the agent's reference throughout the processing steps. |
| **AI Agent**                                | AI Agent              | Core logic node. Directs the workflow using the lead data and executes actions via its available tools. |
| **Tool: Get row(s) in sheet (Google Sheets)**| Google Sheets Tool    | Used by the AI Agent to log or check new leads in a database.               |
| **Tool: Send a message in Gmail**           | Gmail Tool            | Used by the AI Agent to send follow-up emails to the lead or internal notifications. |
| **Tool: Send message and wait (Telegram)**  | Telegram Tool         | Used by the AI Agent for internal team communication or lead qualification steps. |

---

## 3. Detailed Node Configuration

### 3.1. 📝 On form submission (Trigger)
| Parameter       | Value/Configuration                                                                 | Notes                                                                 |
|-----------------|--------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **Form Title**  | Leads Generator                                                                      |                                                                      |
| **Form Description** | Thanks for your interest                                                        |                                                                      |
| **Fields**      | Company Name (Required), Email (Required), What is your monthly budget (in Dollar) (Dropdown) | Budget options include: "100-1000 $", "1000 - 2500 $", and "more then 5000 $". |

---

### 3.2. 🧠 AI Agent (Marketing Logic)
- **System Role/Logic:** Not explicitly visible, but implied through tool usage.  

| Role/Logic        | Tools Used            | Action                                                                 |
|-------------------|-----------------------|------------------------------------------------------------------------|
| **Lead Logging**  | Google Sheets Tool    | Logs the form data (Company Name, Email, Budget) into the Google Sheet database. |
| **Auto Follow-up**| Gmail Tool            | Sends an immediate, personalized "Thank You" or "Next Steps" email to the lead’s submitted address. |
| **Notification**  | Telegram Tool         | Notifies a sales/marketing team member about high-value leads (e.g., "more then 5000 $"). |

---

## 4. Deployment and Setup Notes

### **AI Model:** Workflow uses the **Gemini 2.5 Pro** chat model.  

### Required Credentials
All tools require credentials configured in your n8n instance. Replace with your own values:

| Service              | Credential Name/Type       | Placeholder        |
|----------------------|----------------------------|--------------------|
| Google Gemini (PaLM) | AI Model API Key           | `YOUR_CREDENTIALS` |
| Google Sheets        | OAuth2 / Service Account   | `YOUR_CREDENTIALS` |
| Gmail                | OAuth2 / API Key           | `YOUR_CREDENTIALS` |
| Telegram             | API Key                    | `YOUR_CREDENTIALS` |

### Google Sheet Setup
Ensure the target Google Sheet exists and contains the following columns to support logging:  
- **Company Name**  
- **Email**  
- **Monthly Budget**  

---
