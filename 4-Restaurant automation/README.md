# Kabsah Gulf (Restaurant Automation)

<img width="941" height="573" alt="image" src="https://github.com/user-attachments/assets/df78f931-0dc8-4780-ad18-502f513d2e12" />


## 1. Overview

| Attribute       | Value                                                                 |
|-----------------|----------------------------------------------------------------------|
| **Workflow Name** | Kabsah Gulf (Telegram) with Daily Total Reminder                     |
| **Purpose**       | To provide a Telegram-based sales chatbot experience for customers, log confirmed orders, and run a scheduled daily report to the owner on total earnings and the most ordered item. |
| **Triggers**      | 1. Telegram Message (Customer Order)<br>2. Schedule Trigger (Daily Report) |
| **Dependencies**  | Telegram, Google Gemini Chat Model, Calculator, Gmail, Google Sheets |


## 2. Part 1: Telegram Order Processing (AI Chatbot)

`This part of the workflow handles customer interaction and order logging.
`
### 2.1. Node Configuration Summary (Order Path)

| Node Name             | Type                    | Description                                                                 |
|-----------------------|-------------------------|-----------------------------------------------------------------------------|
| Telegram Trigger      | Telegram Trigger        | Primary Trigger. Starts the workflow when a user sends a message on Telegram. |
| Google Gemini Chat Model | Chat Model           | Provides the language model used by the AI Agent.                           |
| Simple Memory         | Memory Buffer Window    | Maintains conversational history within the AI Agent for each user (sessionKey = Telegram Chat ID). |
| Calculator            | Tool                    | Used by the AI Agent to calculate the total price of the customer's order.  |
| Gmail_Sent            | Gmail Tool              | Used by the AI Agent to send an email to the kitchen/management upon confirmed order. |
| Orders_Data           | Google Sheets Tool      | Used by the AI Agent to append confirmed customer order details to the orders sheet. |
| AI Agent              | AI Agent                | Core logic node. Manages the conversation, utilizes tools, and generates responses based on the System Message. |
| Send a text message   | Telegram                | Sends the AI Agent's response back to the customer on Telegram.             |


### 2.2. AI Agent System Message & Business Logic

`The agent is configured with detailed instructions to function as a sales chatbot:`

| Parameter    | Value/Configuration                                        | Details                                                                 |
|--------------|-------------------------------------------------------------|-------------------------------------------------------------------------|
| **Role**     | Sales chatbot for Kabsah Gulf food restaurant.              | Polite, respectful, and maintains the customer's language/accent.       |
| **Menu Items** | Meat Kabsah (12.00 JD), Chicken Kabsah (9.00 JD), Fries (1.25 JD), Soda Can (0.40 JD). | Pricing is hardcoded into the system message.                           |
| **Location** | Amman, Abu Nusair                                          | The agent is instructed to always ask for the customer's location.      |
| **Tools Usage** | Calculator (Total Price), Gmail_Sent (Email to Kitchen), Orders_Data (Log to Google Sheet). | Tools are triggered by the agent when the order is confirmed and customer details (Name, Phone Number) are collected. |
| **Steps**    | 1. Chat until order confirmation, collecting items, name, and phone number.<br>2. Send Email to kitchen via Gmail_Sent.<br>3. Append data (Name, Order, Total Price) to Google Sheet via Orders_Data. | Defines the business logic flow of the chatbot. |



### 2.3. Data Logging Configuration (Orders_Data Node)

| Parameter      | Value/Configuration                                          | Notes                                                                 |
|----------------|---------------------------------------------------------------|----------------------------------------------------------------------|
| **Operation**  | append                                                        | Adds a new row for each confirmed order.                             |
| **Columns Mapped** | Date (={{ $now }}), Name, Order, Total                    | Name, Order, and Total are populated dynamically by the AI Agent (`$fromAI()`). |



## 3. Part 2: Daily Sales Reporting (Scheduled Reminder)

This part of the workflow is designed to run automatically for management reporting.

### 3.1. Node Configuration Summary (Report Path)

| Node Name              | Type                    | Description                                                                 |
|------------------------|-------------------------|-----------------------------------------------------------------------------|
| Schedule Trigger       | Schedule Trigger        | Secondary Trigger. Starts the daily reporting process (runs daily).         |
| Get row(s) in sheet in Google Sheets1 | Google Sheets Tool | Tool provided to the AI Agent to retrieve all orders logged for calculation. |
| Calculator1            | Tool                    | Tool provided to the AI Agent to perform total earnings and item count calculations. |
| Send a message in Gmail | Gmail Tool             | Tool provided to the AI Agent to send the final report email to the owner. |
| AI Agent1              | AI Agent                | Secondary Agent. Calculates total daily earnings, determines the most ordered item, and emails the owner. |




###  3.2. Daily Report Logic
The primary AI Agent's instructions stipulate that the final step is: "Then extract from the attached sheet, then calculate the total earned at the whole day, with which most ordered item as email to the owner."

- The Schedule Trigger connects to AI Agent1 which is provided with:

- Google Sheets Tool (Get row(s) in sheet in Google Sheets1) to access the order data.

- Calculator Tool (Calculator1) for data analysis.

- Gmail Tool (Send a message in Gmail) to send the compiled report.

- The agent's role is to automate the extraction, calculation, analysis, and final email report based on the data in the Kabsah Gulf sheet.


## 4. Deployment and Setup Notes
AI Model: The workflow uses the models/gemini-2.5-pro chat model.

Required Credentials:

- Telegram API: (YOUR_CREDENTIALS)

- Google Gemini(PaLM) API: (YOUR_CREDENTIALS)

- Gmail OAuth2: (YOUR_CREDENTIALS)

- Google Sheets OAuth2 API: (YOUR_CREDENTIALS)

- Google Sheet: The primary sheet for order logging and reporting must exist with the ID `YOUR_Document` and have the required header columns: Date, Name, Order, and Total.

Note: The Sticky Note explicitly warns: Need to add the sheet tools and create the sheet on google drive to make it done well. This confirms that the Google Sheet must be pre-configured.

