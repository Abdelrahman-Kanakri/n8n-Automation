# Job Application Evaluation

<img width="1074" height="559" alt="image" src="https://github.com/user-attachments/assets/2507d7f6-6c4d-4b54-8cd4-23cdbd82d7a7" />

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Application Evaluation                                               |
| **Purpose**       | Automates screening and evaluation of job applicants. Captures applicant data and resume, uses an AI Agent to extract and assess qualifications, and logs the evaluation summary to a Google Sheet. |
| **Trigger**       | Web Form Submission                                                  |
| **Dependencies**  | Google Gemini Chat Model, Google Sheets, Google Drive                |
| **Version ID**    | a906ff7c-2b5d-4951-a20c-c76b713ff037                                |

---

## 2. Node Configuration Summary
| Node Name                        | Type                  | Description                                                                 |
|----------------------------------|----------------------|-----------------------------------------------------------------------------|
| **On form submission**           | Form Trigger          | Primary Trigger. A public web form collecting applicant's name, email, and resume file. |
| **Upload file**                  | Google Drive          | Saves the uploaded resume into Google Drive for permanent storage.         |
| **Extract from File**            | Google Drive          | Retrieves text content from the uploaded resume for AI Agent analysis.     |
| **Google Gemini Chat Model**     | Chat Model            | Provides the language model (`models/gemini-2.5-pro`) for the AI Agent.    |
| **AI Agent**                     | AI Agent              | Receives resume text and applicant details, evaluates the candidate, and prepares data for logging. |
| **Tool: candidates**             | Google Sheets Tool    | Logs applicant details and evaluation summary.                              |

---

## 3. Detailed Node Configuration

### 3.1. 📝 On form submission (Trigger)

| Parameter         | Value/Configuration                                                                 | Notes                        |
|-------------------|--------------------------------------------------------------------------------------|-------------------------------|
| **Form Title**    | Candidates                                                                          |                               |
| **Fields**        | What is your name? (Required), What is your Email? (Required), Attach your Resume (File Upload, Required) | Defines data captured from the applicant. |

---

### 3.2. 💾 File Handling Nodes
| Node Name         | Parameter   | Configuration | Notes                                              |
|------------------|-------------|---------------|--------------------------------------------------|
| **Upload file**   | Drive ID    | My Drive      | Stores the file in the designated Google Drive location. |
| **Extract from File** | Input Data | File content stream from form submission | Crucial step to extract resume text for AI Agent analysis. |

---

### 3.3. 🧠 AI Agent (Evaluation Logic)

| Role/Logic         | Input Data                                           | Action                                                                 |
|-------------------|------------------------------------------------------|------------------------------------------------------------------------|
| **Resume Analysis** | Applicant's Name, Email, and extracted resume text | Evaluate candidate's fitness for the job based on resume content.      |
| **Evaluation Summary** | Google Sheets Tool                               | Log summarized evaluation (key skills, experience, rating, fit score) to candidates Google Sheet. |

---

### 3.4. 🗃️ Tool: candidates (Data Logging)

| Parameter         | Value/Configuration                                              | Notes                                                     |
|------------------|------------------------------------------------------------------|-----------------------------------------------------------|
| **Sheet Tool**    | candidates                                                      | Configured Google Sheets Tool for logging.               |
| **Data Logged**   | Name, Email, Resume URL (from Upload file step), AI Evaluation/Summary | Columns must be pre-configured in the Google Sheet.      |

---

## 4. Deployment and Setup Notes

- **AI Model:** Gemini 2.5 Pro chat model.  
- **File Processing:** Upload file and Extract from File nodes depend on correct handling of the form’s Resume field.  

### Required Credentials
| Service              | Credential Name/Type       | Placeholder        |
|----------------------|----------------------------|-------------------|
| Google Gemini (PaLM) | AI Model API Key           | `YOUR_CREDENTIALS`|
| Google Drive         | OAuth2 / Service Account   | `YOUR_CREDENTIALS`|
| Google Sheets        | OAuth2 / Service Account   | `YOUR_CREDENTIALS`|

### Usage
The workflow is triggered automatically when an applicant submits the job application form. Evaluation results are stored in the configured Google Sheet.
