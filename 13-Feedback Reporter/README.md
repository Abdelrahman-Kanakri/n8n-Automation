# Feedback Reporter Agent

<img width="758" height="637" alt="image" src="https://github.com/user-attachments/assets/e366b0c4-9759-4192-95cb-fdd8acc70674" />

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Abdelrahman Belal Kanakri Exam project 1                             |
| **Purpose**       | Dual-purpose workflow: 1) Real-time feedback analysis after a customer form submission, 2) Scheduled weekly report generation of sales/category data. |
| **Triggers**      | 1. Web Form Submission (Customer Feedback) <br> 2. Schedule Trigger (Weekly Report) |
| **Dependencies**  | Google Gemini Chat Model (x2), Google Sheets (x2 tools), Gmail       |
| **Version ID**    | 6c476595… (internal)                                                 |

---

## 2. Part 1: Real-Time Feedback Analysis
This path is triggered by a customer filling out a feedback form and uses an AI Agent to immediately process and summarize the response.

### 2.1. Node Configuration Summary (Feedback Path)
| Node Name                  | Type           | Description                                                             |
|-----------------------------|----------------|-------------------------------------------------------------------------|
| **On form submission**      | Form Trigger   | Primary trigger. Collects Name, Phone Number, and Feedback text.        |
| **Google Gemini Chat Model**| Chat Model     | Provides language model for feedback analysis agent.                   |
| **AI Agent**                | AI Agent       | Feedback summarizer: analyzes sentiment and writes a professional summary. |
| **Tool: Weekly Report**     | Google Sheets Tool | Logs original feedback, customer details, and AI summary.            |

### 2.2. Feedback Form Details
| Parameter       | Value/Configuration                        | Notes                                      |
|-----------------|--------------------------------------------|--------------------------------------------|
| **Form Title**  | Thank you for eating with us               |                                            |
| **Form Description** | Give me your feedback about our service |                                            |
| **Fields**      | Name, Phone Number, Feedback (textarea), Hidden Field (={{$now}}) | Hidden field ensures timestamp is recorded with every submission. |

---

## 3. Part 2: Weekly Business Reporting
This path is triggered on a schedule and focuses on business intelligence, analyzing sales categories.

### 3.1. Node Configuration Summary (Report Path)
| Node Name                  | Type          | Description                                                             |
|-----------------------------|---------------|-------------------------------------------------------------------------|
| **Schedule Trigger**        | Schedule Trigger | Primary trigger. Runs report generation weekly.                         |
| **Google Gemini Chat Model1** | Chat Model  | Provides language model for the reporting agent.                        |
| **AI Agent1**               | AI Agent       | Reporting agent: accesses category data, analyzes sales trends, compiles report. |
| **Tool: Get_Categories**    | Google Sheets Tool | Fetches raw sales and category data from sheet.                      |
| **Send a message**          | Gmail          | Sends the final weekly report via email.                                |

### 3.2. Reporting Logic
The reporting agent's instructions:

1. **Access Data:** Use `Get_Categories` tool to pull weekly sales/category data.  
2. **Analyze:** Determine total sales, best-performing categories, and worst-performing categories.  
3. **Report:** Compile a detailed summary and send via Gmail.  

---

## 4. Deployment and Setup Notes

- **AI Model:** Uses Gemini chat model for both analysis and reporting.  
- **Google Sheets Setup:** Two separate sheets required:  
  1. Logging customer feedback (`Weekly Report` tool).  
  2. Sales category data (`Get_Categories` tool).  

### Required Credentials
| Service                        | Credential Name/Type         | Placeholder           |
|--------------------------------|-----------------------------|---------------------|
| Google Gemini (PaLM)           | AI Model API Key            | `YOUR_CREDENTIALS`  |
| Google Sheets (x2)             | OAuth2 / Service Account    | `YOUR_CREDENTIALS`  |
| Gmail                          | OAuth2 / API Key            | `YOUR_CREDENTIALS`  |
| Form Trigger                   | Webhook                     | `YOUR_CREDENTIALS`  |
