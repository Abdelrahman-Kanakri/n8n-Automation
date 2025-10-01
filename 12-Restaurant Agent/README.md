# Restaurant Agent
<img width="1074" height="705" alt="image" src="https://github.com/user-attachments/assets/c6da3edb-af5d-41a9-a228-fd228937892b" />


# n8n Workflow Documentation: Restaurant Agent

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Restaurant Agent                                                     |
| **Purpose**       | Acts as a comprehensive multi-agent customer service system for a restaurant. Routes incoming chat messages to specialized AI Agents capable of taking new orders, checking status, tracking delivery, or processing cancellations. |
| **Trigger**       | Chat Message Received                                                 |
| **Dependencies**  | Google Gemini Chat Model (x5), Google Sheets (x4 tools), Calculator (x1 tool) |
| **Version ID**    | 3f92f252… (internal)                                                 |

---

## 2. Node Configuration Summary
| Node Name                   | Type                     | Description                                                                 |
|------------------------------|--------------------------|-----------------------------------------------------------------------------|
| **Simple Memory**            | Memory Buffer Window     | Maintains chat history for main agent and all sub-agents.                  |
| **Google Gemini Chat Model** | Chat Model (x5)          | Provides language model for Main Agent and four specialized sub-agents.    |
| **Main Agent**               | AI Agent                 | Routes messages, classifies customer intent, and passes control to sub-agents. |
| **Order Taker**              | AI Agent                 | Handles new food orders and logs them to the sheet.                         |
| **Status Checker**           | AI Agent                 | Handles inquiries about order status.                                       |
| **Delivery**                 | AI Agent                 | Handles delivery tracking questions.                                        |
| **Cancelation**              | AI Agent                 | Handles order modification/cancellation requests.                           |
| **Calculator**               | Tool                     | Used by Order Taker to calculate final price.                               |
| **Google Sheets Tools (x4)** | Google Sheets Tool       | Provides data access/mutation capabilities to agents (e.g., log order, update status). |
| **Send a text message**      | Telegram                 | Sends final response from active agent back to the user.                   |

---

## 3. Specialized Sub-Agent Logic & Tools
| Sub-Agent       | Primary Function / Intent            | Dedicated Tools                                                   |
|-----------------|-------------------------------------|-------------------------------------------------------------------|
| **Order Taker** | Take new food orders                 | Calculator (for pricing), Google_Sheets_New_Order (log orders)    |
| **Status Checker** | Check order confirmation/status    | Google_Sheets_Find_Order (retrieve status by order ID)            |
| **Delivery**    | Provide delivery tracking info       | Google_Sheets_Find_Order (retrieve delivery status/ETA)           |
| **Cancelation** | Cancel or modify existing orders     | Google_Sheets_Find_Order, Google_Sheets_Update_Status (mark canceled), Email_Send_Cancellation_Notice (notify kitchen/customer) |

---

## 4. Deployment and Setup Notes

- **AI Model:** Gemini 2.5 Pro chat model across all agents.  
- **Tool Configuration:** Four distinct Google Sheets tools + one Calculator tool must be correctly configured and connected to agents.  

### Required Credentials
| Service                        | Credential Name/Type         | Placeholder           |
|--------------------------------|-----------------------------|---------------------|
| Google Gemini (PaLM)           | AI Model API Key            | `YOUR_CREDENTIALS`  |
| Google Sheets (x4)             | OAuth2 / Service Account    | `YOUR_CREDENTIALS`  |
| Gmail                          | OAuth2 / API Key            | `YOUR_CREDENTIALS`  |
| Chat Trigger                   | Webhook / platform API (e.g., Telegram) | `YOUR_CREDENTIALS` |

- **Usage:** The system is initiated whenever a message is received on the configured chat platform (e.g., Telegram).
