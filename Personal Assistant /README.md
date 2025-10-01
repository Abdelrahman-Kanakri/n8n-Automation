# Personal Assistant (Voice Chat)
<img width="1511" height="550" alt="image" src="https://github.com/user-attachments/assets/04919e52-42ab-43c5-97c1-7367894bbbcc" />


## 1. Overview

| Attribute     | Value                                                                 |
|---------------|----------------------------------------------------------------------|
| **Workflow Name** | PersonalAssist (Voice Assist)                                      |
| **Purpose**       | To function as a voice-activated personal assistant over Telegram, capable of managing emails, scheduling calendar events, and general conversation using AI and a set of predefined tools. |
| **Triggers**      | Telegram Update (Voice Message or Text)                            |
| **Dependencies**  | Telegram, Google Gemini Chat Model, Gmail, Google Sheets, Google Calendar |
| **Version ID**    | e1d4d3d2-d177-4581-9b16-953b1b9ff7c4                               |


## 2. Node Configuration Summary

| Node Name                              | Type              | Description                                                                 |
|----------------------------------------|-------------------|-----------------------------------------------------------------------------|
| **Telegram Trigger**                   | Telegram Trigger  | Primary Trigger. Starts the workflow on any incoming message.               |
| **Get a file**                         | Get file          | Downloads the audio file if the incoming message is a voice note.           |
| **Analyze audio**                      | Analyze audio     | Uses the Gemini model to convert the voice recording to text (Speech-to-Text). |
| **AI Agent**                           | AI Agent          | Core logic and brain of the assistant. Manages conversation, memory, and tool routing. |
| **Switch1**                            | Switch            | Routes the final output: Sends audio if Text-to-Speech is enabled, otherwise sends text. |
| **Generate audio**                     | Generate audio    | Converts the AI Agent's text response back into an audio file (Text-to-Speech). |
| **Send an audio file / Send a message** | Telegram          | Sends the final voice or text response back to the user on Telegram.        |


## 3. Detailed AI Agent Configuration

### 3.1. Agent Persona and Behavior

| Parameter | Value/Configuration | Details |
|-----------|----------------------|---------|
| **Role** | PersonalAssistant | A personal assistant with access to multiple tools. |
| **Behavior** | Responds concisely, succinctly, and in a friendly, approachable style. | Configured to be helpful and brief in tone. |
| **Context** | Includes the current date and time (`{{ $now }}`) in its internal instructions. | Ensures accuracy for time-sensitive tasks (e.g., scheduling, reminders). |



### 3.2. Agent Tools (Skills)

The AI Agent has access to the following specialized tools to fulfill user requests:

| Tool Name       | Service         | Purpose                                                                 |
|-----------------|-----------------|-------------------------------------------------------------------------|
| **Gmail_Summary** | Gmail          | Summarizes email contents for the user.                                 |
| **Gmail_Sent**    | Gmail          | Sends new emails on behalf of the user.                                |
| **Contacts_Sheet**| Google Sheets  | Looks up the email address of a contact before sending an email.        |
| **Calendar_Set**  | Google Calendar| Schedules new appointments, always ensuring a proper title is included. |
| **Calendar_Get**  | Google Calendar| Retrieves existing events and informs the user of upcoming appointments.|


## 4. Deployment and Setup Notes

- **AI Model**: The workflow uses the **Gemini 2.5 Pro** chat model.  
- **Voice Processing**: Incoming Telegram voice messages are transcribed into text, processed by the AI Agent, and then converted back into audio files (via the **Generate audio** node) to complete the voice loop.  

### Required Credentials
This workflow requires multiple OAuth2 credentials and API keys. Replace the placeholders below with your configured credential names in your n8n instance:

| Service             | Credential Name/Type |
|---------------------|-----------------------|
| **Telegram**        | API Key (Bot Token)   |
| **Google Gemini**   | API Key               |
| **Gmail**           | OAuth2                |
| **Google Sheets**   | OAuth2                |
| **Google Calendar** | OAuth2                |

