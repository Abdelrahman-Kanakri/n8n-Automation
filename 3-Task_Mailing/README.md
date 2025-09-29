# 3 Task Mailing
<img width="985" height="416" alt="image" src="https://github.com/user-attachments/assets/8efac49d-beb0-4d6d-b857-0898ab75dfbc" />


## 1. Overview

| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Mail Sending to Separate Employees                                    |
| **Purpose**       | To retrieve a list of tasks from a shared Google Sheet daily, filter the tasks based on assignment, and send a personalized task list email to each of two separate employees (Shafee and Spong). |
| **Trigger**       | Daily Schedule                                                       |
| **Dependencies**  | Gmail, Google Sheets                                                 |
| **Version ID**    | b00113a8-7443-4d27-b446-7c2019b885e9                                 |




## 2. Node Configuration Summary

| Node Name          | Type           | Description                                                                 |
|--------------------|----------------|-----------------------------------------------------------------------------|
| Schedule Trigger   | Schedule Trigger | Trigger Node. Starts the workflow at 7:50 AM daily.                          |
| Get row(s) in sheet | Google Sheets   | Retrieves all task data. (Used for Shafee's path)                           |
| Get row(s) in sheet1| Google Sheets   | Retrieves all task data. (Used for Spong's path)                            |
| Filter             | Filter          | Filters the data to include only tasks assigned to "Shafee".                |
| Filter1            | Filter          | Filters the data to include only tasks assigned to "Spong".                 |
| Edit Fields1       | Set             | Formats Shafee's filtered task list into a single email body variable (All Tasks). |
| Edit Fields        | Set             | Formats Spong's filtered task list into a single email body variable (All Tasks). |
| Send a message     | Gmail           | Sends the daily task email to Shafee.                                       |
| Send a message1    | Gmail           | Sends the daily task email to Spong.                                        |


## 3. Detailed Node Configuration

### 3.1. ⏰ Schedule Trigger (Trigger)

| Parameter | Value/Configuration | Notes                                                                 |
|-----------|----------------------|----------------------------------------------------------------------|
| Rule (Interval) | Time: 7:50 | The workflow executes once daily at 7:50 AM. It then branches out to simultaneously fetch data for both employees. |


### 3.2. 📂 Data Fetch & Filter Path 

| Node              | Parameter      | Value/Configuration                                                                 | Notes                                                                 |
|-------------------|----------------|--------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| Get row(s) in sheet | Document ID    | `DocumentID`                                         | Retrieves all rows from the shared "Task Spongbob" sheet.            |
| Filter            | Condition      | `{{ $json['Assigned to'] }} equals "Shafee"`                                         | Passes only the rows where the task is assigned to Shafee.            |
| Edit Fields1 (Set)| Variable       | All Tasks                                                                            | Uses a JavaScript expression to combine all filtered tasks into one string, separated by `<br>` (HTML line break). |
| Send a message    | Send To        | employeeEmail@gmail.com                                                      | Hardcoded recipient (likely a placeholder or proxy for Shafee).       |
|                   | Message        | Includes `{{ $json['All Tasks'] }}` and signs off as "Shafee".                       | Uses the compiled task list.                                          |

## 4. Deployment and Setup Notes
- Shared Google Sheet: The workflow relies on the sheet with ID of your `Document` having a column titled "Assigned to" to successfully filter the tasks.

- Efficiency Note: The workflow retrieves all rows from the Google Sheet twice at the start. For efficiency in future updates, consider fetching the data once and using the Split In Batches node before applying two separate Filter nodes.

- Credentials: The required Gmail and Google Sheets OAuth2 credentials (YOUR_CREDENTIALS) must be configured in n8n.

