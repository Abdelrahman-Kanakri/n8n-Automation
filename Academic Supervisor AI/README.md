# Academic Supervisor AI
<img width="1023" height="567" alt="image" src="https://github.com/user-attachments/assets/1bfd4f08-d6f2-487a-9aa2-660cf3b14af3" />

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|----------------------------------------------------------------------|
| **Workflow Name** | Academic Supervisor AI                                               |
| **Purpose**       | To act as a virtual academic advisor that helps students build a personalized, balanced study plan and course selection for the upcoming semester, referencing the Idaho State University course catalog. |
| **Trigger**       | Telegram Message                                                    |
| **Dependencies**  | Google Gemini Chat Model, Telegram                                  |
| **Version ID**    | dcde31a6-bfcf-4750-b553-9baaaeb220e7                                |


## 2. Node Configuration Summary

| Node Name            | Type                  | Description                                                                 |
|-----------------------|-----------------------|-----------------------------------------------------------------------------|
| Telegram Trigger      | Telegram Trigger      | Trigger Node. Starts the conversation when a student sends a message on Telegram. |
| Google Gemini Chat Model | Chat Model         | Provides the language model (`models/gemini-2.5-pro`) for the AI Agent.     |
| Simple Memory         | Memory Buffer Window  | Maintains the chat history for each student using their chat ID as the session key. |
| AI Agent              | AI Agent              | Core conversational and logic node that follows the academic advising protocol. |
| Send a text message   | Telegram              | Sends the AI Agent's advice and questions back to the student on Telegram.  |


## 3. Academic Advisor Logic (AI Agent System Message)

The AI Agent is configured with a multi-step protocol to ensure a comprehensive academic plan is generated.

### Key Steps and Requirements

| Step                  | Requirement                                                                 | Notes                                                                 |
|-----------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **Information Collection** | Collect Major, Current Academic Year, and completed/in-progress courses. | Must validate inputs and ask for missing information before proceeding. |
| **Academic Analysis** | Identify core/required courses and check prerequisites for each course.     | Exclude courses the student cannot register for yet.                  |
| **Study Plan Proposal** | Propose a list of recommended courses (core + electives).                  | The plan must be balanced (credit hours, course diversity), with short notes explaining why each course is recommended. |
| **Course Tree**       | Build an academic course pathway (Course Tree) showing hierarchical relationships (prerequisites/follow-up courses). | Helps students understand long-term academic planning.                |
| **Reference Source**  | Must use Idaho State University’s official course catalog as the primary reference for all recommendations. | Ensures recommendations are accurate and official.                     |
| **Final Confirmation**| Chat until the student confirms their course selections, and collect their full name and contact information (phone number or student ID). | Completes the advising workflow.                                      |




4. Deployment and Setup Notes
- AI Model: The workflow uses the Gemini 2.5 Pro chat model.

- Memory: The Simple Memory node uses the Telegram Chat ID (={{ $json.message.chat.id }}) to ensure the AI Agent remembers the context of the conversation for each individual student.

- Required Credentials: This workflow requires credentials for the Telegram API and the Google Gemini (PaLM) API.

- Usage: The workflow is triggered by an incoming message to the linked Telegram bot.
