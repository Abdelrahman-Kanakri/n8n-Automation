# Hotel Customer Feedback Summerizer

<img width="602" height="446" alt="image" src="https://github.com/user-attachments/assets/a1821ffe-c988-4c7d-9f96-9978ecdfe7f9" />

## 1. Overview

| Attribute       | Value                                                                 |
|-----------------|----------------------------------------------------------------------|
| **Workflow Name** | Hotel Feedback Summarizer                                            |
| **Purpose**       | To collect customer feedback via a web form, use an AI agent to analyze and summarize the review's sentiment, and automatically save the summary and contact details to Airtable. |
| **Trigger**       | Web Form Submission                                                 |
| **Dependencies**  | Google Gemini Chat Model, Airtable                                  |
| **Version ID**    | 32d145dd-0735-4a92-b036-9008390c6cf8                                |


## 2. Node Configuration Summary

| Node Name                  | Type                  | Description                                                                 |
|-----------------------------|-----------------------|-----------------------------------------------------------------------------|
| On form submission          | Form Trigger          | Trigger Node. Collects customer's name and review text via a public web form. |
| Google Gemini Chat Model    | Chat Model            | Provides the language model used by the AI Agent.                           |
| Simple Memory               | Memory Buffer Window  | Stores the submitted review content for the agent's reference.              |
| Create a record in Airtable | Airtable Tool         | Tool used by the AI Agent to log the original and summarized data.          |
| AI Agent                    | AI Agent              | Core logic. Processes the review, analyzes sentiment, creates the summary, and uses the Airtable tool to log the result. |

## 3. Detailed Node Configuration

### 3.1. 📝 On form submission (Trigger)

| Parameter        | Value/Configuration | Notes                                                                 |
|------------------|----------------------|----------------------------------------------------------------------|
| **Form Title**   | Hotel Feedback       | Title displayed on the web form.                                     |
| **Form Description** | Thank you for taking our hotel. If you please write a feedback of your staying in the hotel | Shown as a description to guide customers before submitting feedback. |
| **Fields**       | First Name (Required), Last Name (Required), Review (Text Area) | Collects customer details and review text.                          | Defines the required customer input. |

### 3.2. 🧠 AI Agent (Processing & Summarization)

| Parameter        | Value/Configuration                                                                 | Details                                                                 |
|------------------|--------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **System Message** | # You are a summarizer specialist. Your task is to process resident feedback and save a clean summary to Airtable. | Defines the agent's professional persona and core goal. |
| **Input Steps**    | 1. Extract Name and Review from form data.<br>2. Analyze the review’s sentiment (positive, negative, or neutral).<br>3. Write a concise summary reflecting the resident’s opinion/tone.<br>4. Append First Name, Last Name, and Summarized review to Airtable under the "Summery" field. | Ensures the agent is professional, neutral, and saves only the clean summary text. |


### 3.3. 🗃️ Create a record in Airtable (Tool)

| Parameter         | Value/Configuration                  | Notes                                                                 |
|-------------------|---------------------------------------|----------------------------------------------------------------------|
| **Operation**     | create                               | Creates a new record for each form submission.                       |
| **Base**          | n8n hotel                            | Target Airtable Base (ID hidden for security).                        |
| **Table**         | Table 2                              | Target Airtable Table (ID hidden for security).                       |
| **Column Mapping**| First Name, Last Name, Review, Summery | All values are populated dynamically by the AI Agent (`$fromAI()`).   |
| **Credential**    | Airtable account (via Personal Access Token) | Sensitive details removed for security.                               |




## 4. Deployment and Setup Notes
- AI Model: The workflow uses the models/gemini-2.5-pro chat model.

- Airtable: The target Airtable Base (YOUR_CREDENTIALS) and Table (YOUR_CREDENTIALS) must be set up with the four necessary field headers: First Name, Last Name, Review, and Summery.

- Form URL: Once this workflow is active, the On form submission node will provide a public URL for users to submit feedback.

