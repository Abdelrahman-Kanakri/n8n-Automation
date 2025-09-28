# Task Sender

<img width="1009" height="479" alt="image" src="https://github.com/user-attachments/assets/7729bf05-61ea-4010-87e2-e269e1b6cddc" />


## 1. Overview

| Attribute         | Value                                                                                                                                         |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| **Workflow Name** | Second Automation                                                                                                                             |
| **Purpose**       | To retrieve a list of tasks from a Google Sheet at a scheduled time each day, format the data into a readable message body, and send a daily task reminder email to a specific user. |
| **Trigger**       | Daily Schedule                                                                                                                                |
| **Dependencies**  | Gmail, Google Sheets                                                                                                                          |



## 2. Node Configuration Summary

| Node Name            | Type            | Description                                                                 |
|----------------------|-----------------|-----------------------------------------------------------------------------|
| **Schedule Trigger** | Schedule Trigger | Trigger Node. Starts the workflow at a specified daily time.                 |
| **Get row(s) in sheet** | Google Sheets  | Retrieves all task rows from the specified Google Sheet.                     |
| **Code in Python (Beta)** | Code        | Processes the retrieved sheet data into a single, formatted HTML/text string suitable for the email body. |
| **Send a message**   | Gmail           | Sends the compiled daily task list email to the designated recipient.        |



## 3. Detailed Node Configuration

### 3.1 ⏰ Schedule Trigger (Trigger)


| Parameter        | Value/Configuration | Notes                                                                 |
|------------------|---------------------|----------------------------------------------------------------------|
| **Rule (Interval)** | Time: 7:50          | The workflow executes once daily at 7:50 AM (based on the n8n server's timezone). |



### 3.2. 📊 Get Row(s) in Sheet (Data Retrieval)


| Parameter     | Value/Configuration                                               | Notes                                      |
|---------------|------------------------------------------------------------------|--------------------------------------------|
| Operation     | Get all (Default)                                                 | Retrieves all rows of data from the sheet |
| Sheet Name    | Sheet1 (gid=0)                                                    |                                            |




### 3.3. 🐍 Code in Python (Data Formatting)


| Language | Purpose                                                                                                          | Output Variable |
|----------|------------------------------------------------------------------------------------------------------------------|----------------|
| Python   | Iterates over each row retrieved from the Google Sheet and constructs an HTML-formatted string (`<br>` for new lines) for the email body, using the columns `row_number`, `Task`, and `Time`. |  







### 3.4. 📧 Send a Message (Email Notification)


| Parameter     | Value/Configuration                                           | Notes                                                                                 |
|---------------|---------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Credentials   | Gmail account (YOUR_CREDENTIALS)                              | OAuth2 credential for sending emails                                               
| Send To       | youremail@oraganization_domain                                | Hardcoded recipient for the daily task list                                       |
| Subject       | New Task                                                      |                                                                                   |
| Message       | `=You have to do the following:<br>\n{{ $json.emailBody }}` | Uses the dynamically generated HTML string (`emailBody`) from the previous node as the email content |


## 4. Deployment and Setup Notes

- **Workflow File:**  
  The JSON code for this workflow is named `2- Second Automation.json` in this project.

- **Credentials:**  
  The workflow requires two OAuth2 credentials to be set up in your n8n instance:
  - **Gmail OAuth2:** Named Gmail account (ID: YOUR_CREDENTIALS) with the necessary scopes to send emails.
  - **Google Sheets OAuth2:** Named Google Sheets account (ID: YOUR_CREDENTIALS) with scopes to manage spreadsheets.

- **Google Sheet Structure:**  
  The target Google Sheet (`SheetID`) must have columns that match the keys being used in the Python code for correct data extraction (e.g., `row_number`, `Task`, `Time`).


