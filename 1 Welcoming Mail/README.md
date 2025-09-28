# Welcoming Email

<img width="875" height="611" alt="Screenshot 2025-09-28 080240" src="https://github.com/user-attachments/assets/68175e29-8bad-495b-b35e-ca139403324b" />


## 1. Overview

| Attribute      | Value                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| **Workflow Name** | Welcoming Email                                                                                                                         |
| **Purpose**        | To automate the process of collecting new customer information via a web form, sending a personalized welcome email, notifying an internal team member, and logging the data to a Google Sheet. |
| **Trigger**        | Web Form Submission                                                                                                                   |
| **Dependencies**   | Gmail, Google Sheets                                                                                                                  |
| **Version ID**     | `b90480f9-6fe2-446d-81f6-2f4f7f7e24a1`                                                                                                 |


## 2. Workflow Description 
This n8n workflow is designed to manage new customer registrations. It is triggered when a user completes and submits a public web form. The collected data is then processed in parallel to:

* Send a personalized welcome email to the new customer.

* Notify an administrator about the new registration.

* Append the customer data to a designated Google Sheet for record-keeping.

## 3. Workflow Nodes

| Node Name           | Description                                                                      |
|---------------------|----------------------------------------------------------------------------------|
| On form submission      | **Form Trigger** – Creates a public web form to capture new customer data.   |
| Send a message          | **Gmail** – Sends a personalized welcome email to the new customer.          |
| Send a message1         | **Gmail** – Sends an internal notification email to the administrator.       |
| Append row in sheet     | **Google Sheets** – Logs the collected customer data to a specific row.      |



## 4. Detailed Node Configuration

### 4.1. 📧 On form submission (Trigger)

| Parameter        | Value/Configuration                                                                 | Notes                                   |
|------------------|--------------------------------------------------------------------------------------|-----------------------------------------|
| **Form Title**       | New Customer                                                                        |                                         |
| **Form Description** | Welcome to Jordan!                                                                  |                                         |
| **Fields**           | First Name (Required), Last Name (Required), Email (Required), Phone Number, Level? (Dropdown: 1, 2, 3) | Defines the data captured from the user. |



### 4.2. 📥 Send a message (Welcome Email)

| Parameter    | Value/Configuration                                | Notes                                                                 |
|--------------|----------------------------------------------------|----------------------------------------------------------------------|
| **Credentials** | Gmail account (`YOUR_CREDENTIALS`)                 | OAuth2 credential for sending emails. *(Note: This should be configured securely.)* |
| **Send To**     | `={{ $json.Email }}`                               | Dynamically pulled from the trigger form submission.                  |
| **Subject**     | `=Welcome to Jordan {{ $json['First Name'] }}`      | Personalized with the customer's first name.                          |
| **Message**     | `=Welcome to Jordan {{ $json['First Name'] }}, you have been selected to be a new member in a community` | The email body text.                                                  |




### 4.3 🚨 Send a Message1 (Internal Notification)



| Parameter    | Value/Configuration                                                                 | Notes                                        |
|--------------|-------------------------------------------------------------------------------------|----------------------------------------------|
| **Credentials** | Gmail account (`YOUR_CREDENTIALS`)                                                    | Uses the same Gmail credential.              |
| **Send To**     | `abdelrahamankanakrik@gmail.com`                                                    | Hardcoded internal recipient for notification. |
| **Subject**     | `New Member Added`                                                                 |                                              |
| **Message**     | `=New member has been added\n{{ $json['First Name'] }} {{ $json['Last Name']}} <br>\n{{ $json.Email }} <br>\n{{ $json['Phone Number'] }} <br>` | Contains all critical new user data for immediate review. |





## 5. Deployment and Setup Notes
* Workflow File: The JSON code for this workflow is located in the repository at [PATH_TO_JSON_FILE].

* Credentials: The workflow requires two OAuth2 credentials to be set up in your n8n instance:

* Gmail OAuth2: Named Gmail account (ID: YOUR_CREDENTIALS) with the necessary scopes to send emails.

* Google Sheets OAuth2: Named Google Sheets account (ID: YOUR_CREDENTIALS) with scopes to manage spreadsheets.

* Form URL: Once this workflow is activated, the On form submission node will generate a public URL. This URL must be shared with customers to trigger the workflow.

* Google Sheet: Ensure the target Google Sheet exists and has the required header columns: Name, Email and Phone Number.






