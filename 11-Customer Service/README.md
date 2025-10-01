# Customer Service


<img width="1069" height="491" alt="image" src="https://github.com/user-attachments/assets/1c924030-3e60-44a8-9a31-2ea38e8d266c" />


## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Customer Service Agent (Dhafir)                                      |
| **Purpose**       | Acts as an AI-powered customer support agent ('Dhafir') that handles incoming issues, classifies them, generates ticket numbers, logs the history, and routes tickets by email to the correct department. |
| **Trigger**       | Chat Message Received                                                 |
| **Dependencies**  | OpenAI Chat Model, Gmail, Google Sheets (Departments and History)    |
| **Version ID**    | 71250bc6-99bc… (internal)                                            |

---

## 2. Node Configuration Summary
| Node Name                    | Type                  | Description                                                                 |
|-------------------------------|----------------------|-----------------------------------------------------------------------------|
| **When chat message received** | Chat Trigger         | Primary Trigger. Starts workflow on any incoming chat message (Telegram, Slack, or Webhook). |
| **OpenAI Chat Model**         | Chat Model            | Provides language model for AI Agent conversation and classification.      |
| **Simple Memory**             | Memory Buffer Window  | Maintains conversational history for contextual follow-up.                 |
| **AI Agent**                  | AI Agent              | Core logic for ticket processing: questioning, classification, logging, routing. |
| **Tool: Send a message in Gmail** | Gmail Tool       | Sends final ticket notification email to the appropriate department.      |
| **Tool: Departments**         | Google Sheets Tool    | Lookup tool for mapping Issue Type → Department Email Address.            |
| **Tool: History**             | Google Sheets Tool    | Logs details of every created ticket for auditing and tracking.           |

---

## 3. Detailed AI Agent Logic

### 3.1. Agent Persona and Protocol
| Parameter        | Value/Configuration                          | Details                                                       |
|-----------------|----------------------------------------------|---------------------------------------------------------------|
| **Role/Name**   | Customer support agent, named 'Dhafir'      | Professional, courteous, and efficient.                     |
| **Initial Steps** | Ask for customer’s full name and issue description | Critical for data collection before processing.             |
| **Processing Steps** | 1. Classify the problem <br> 2. Generate unique ticket number <br> 3. Log issue to History (Issue, Name, Email, Ticket Number, Department) <br> 4. Send email to appropriate department | Routes classified issue via email after logging.             |

### 3.2. Tools Mapping
| Tool Name    | Type           | Required Data/Notes                                                 |
|-------------|----------------|----------------------------------------------------------------------|
| **Departments** | Google Sheets | Lookup table of Issue Type / Department Name → Department Email.    |
| **History**     | Google Sheets | Columns: Issue, Customer Name, Customer Email, Ticket Number, Department. Logs all generated tickets. |

---

## 4. Deployment and Setup Notes

- **AI Model:** Uses OpenAI Chat Model (specific model version not detailed).  
- **Google Sheets Setup:** Two separate sheets or documents required:  
  1. **Departments** – for department lookup.  
  2. **History** – for ticket logging.  

### Required Credentials
| Service                | Credential Name/Type                  | Placeholder           |
|-----------------------|--------------------------------------|---------------------|
| OpenAI                 | API Key                              | `YOUR_CREDENTIALS`  |
| Gmail                  | OAuth2 / API Key                      | `YOUR_CREDENTIALS`  |
| Google Sheets          | OAuth2 / Service Account              | `YOUR_CREDENTIALS`  |
| Chat Trigger           | Webhook or platform API (Telegram etc.) | `YOUR_CREDENTIALS` |
