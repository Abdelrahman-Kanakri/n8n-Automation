# AI-Powered Data Science Assistant (n8n Workflow)
This repository contains an advanced n8n workflow for an AI-powered Data Science Assistant named AbdoData. This conversational agent can process and analyze data from various sources, answer questions about uploaded documents, and perform database operations on both SQL and NoSQL databases.
<img width="941" height="739" alt="image" src="https://github.com/user-attachments/assets/775a35c5-5d76-4e6e-b851-3740d03b73b0" />

## ✨ Features
* Conversational Interface: Interacts with users via Telegram.

* Multi-Source Data Handling: Processes uploaded files (.pdf, .csv, .xlsx, .txt, .json) and plain text queries.

* Retrieval-Augmented Generation (RAG): Creates a searchable knowledge base from user-uploaded documents using Supabase Vector Store for context-aware answers.

* Database Orchestration: Intelligently delegates tasks to specialized agents for structured (Airtable) and unstructured (MongoDB) database operations.

* Modular Agent Architecture: Uses a main orchestrator agent that controls specialist agents for specific tasks, ensuring clean and efficient logic.
___
# workflow-walkthrough-ai-data-scientist-chatbot
The workflow is divided into four main stages, which are triggered and managed based on user input.

## 1. Workflow Initiation and Input Handling
This section covers the initial trigger and the routing logic based on the type of input received from the user.

* Telegram Trigger: The workflow is initiated when a user sends a message (either text or a file) to the connected Telegram bot.

* Input Type Router (Switch node): This node inspects the incoming message.

* If the message contains a document, it routes the workflow to the file processing and knowledge base creation pipeline.

* If the message is plain text, it routes the workflow directly to the AI Orchestration stage.

* Get File (Telegram node): If a document is detected, this node downloads the file's binary data from Telegram's API to make it available for processing.

## 2. Data Extraction and Standardization
**This stage processes the uploaded file, extracts its content, and transforms it into a standardized text format for the AI.**

* File Type Router (Switch node): Routes the downloaded file to the appropriate extractor based on its extension (.pdf, .csv, etc.).

* Extractor Nodes: A dedicated extractor node for each file type parses the raw file and converts its contents into either structured JSON (from CSV/XLSX) or raw text (from PDF/TXT).

* Standardization (Code nodes):

 <ol>
   <li>Tabular Data: Structured data is converted into a clean Markdown table.</li>

 <li>Non-Tabular Data: Unstructured text is consolidated into a single plain text block.</li>

 <li>Merge Node: This node unifies the output from the different standardization paths into a single data stream, ensuring the rest of the workflow handles the data consistently, regardless of the original file type.</li>
</ol>

## 3. Knowledge Base Creation (RAG Pipeline)
**This pipeline ingests the standardized text into the AI's long-term memory—the Supabase Vector Store.**

* This chain of nodes forms the core of the Retrieval-Augmented Generation (RAG) system:

<ol>
  <li>Text Splitter: The standardized text is broken down into smaller, meaningful chunks.</li>

<li>Embeddings Model (Google Gemini): Each chunk is converted into a numerical vector representation (an embedding).</li>

<li>Vector Store (Supabase): The original text chunks and their corresponding embeddings are stored in the Supabase Vector Store.</li>
</ol>

This process creates a powerful, searchable knowledge base where information can be retrieved based on its semantic meaning, not just keywords.

## 4. AI Orchestration and Task Execution 🤖
This is the conversational core of the workflow, managed by a central AI Agent that acts as an orchestrator. It understands user requests and delegates tasks to a set of specialized tools.

### The Main AI Agent (Orchestrator)
This is the primary "brain" that the user interacts with. Its job is to analyze the user's intent and delegate the task to the correct specialist tool.

#### Tool 1: The RAG System (Knowledge Retriever)
* Node: Supabase Vector Store (in retriever mode)

* Purpose: Serves as the AI's long-term memory for all uploaded documents.

* Function: When a user asks a question about a document, the Main Agent uses this tool to search the vector database for the most relevant text chunks (filtered by the user's chat_id) and uses them as context to form an accurate answer.

#### Tool 2: The NoSQL Specialist (MongoDB Agent)
* Purpose: A dedicated sub-agent that is an expert in handling unstructured or semi-structured data operations in MongoDB.

* Function: If the Main Agent determines a request is for MongoDB (e.g., "insert these user logs"), it passes the task to this specialist.

* Capabilities: This agent has access to a full suite of MongoDB tools: Find, Insert, Delete, Aggregate, and Update.

#### Tool 3: The SQL Specialist (Airtable Agent)
* Purpose: A dedicated sub-agent that is an expert in handling structured, table-based data in Airtable.

* Function: If the Main Agent identifies a request for Airtable (e.g., "create a new task record"), it delegates the task to this specialist.

* Capabilities: This agent can use tools to Get, Search, Create, Update, and Delete records in an Airtable base.

# 🛠️ Setup and Configuration
To use this workflow, you need to import the Data Science Assist2 Last.json file into your n8n instance and configure the necessary credentials.

## Required Credentials
**You will need to create and connect the following credentials in your n8n workspace:**

* Telegram: For the bot trigger and messaging.

In Telegram Trigger & Telegram nodes, set the credential to `YOUR_CREDENTIALS`.

* Google Gemini (PaLM) API: For the language model and embeddings.

In all Google Gemini Chat Model and Embeddings Google Gemini nodes, set the credential to `YOUR_CREDENTIALS`.

* Supabase API: For the vector store.

In the Supabase Vector Store nodes, set the credential to `YOUR_CREDENTIALS`.

* MongoDB: For the NoSQL database.

In all mongoDbTool nodes, set the credential to `YOUR_CREDENTIALS`.

* Airtable API: For the structured database.

In all airtableTool nodes, set the credential to `YOUR_CREDENTIALS`.

#### Node Configuration
* Airtable Nodes: You will need to replace the placeholder Base ID  and Table ID with your own Airtable base and table information.

* Telegram Trigger: Ensure the webhook is correctly registered and active.


# Full Node Explanation: 
| Node Name / Type | Category | Purpose in Workflow |
| :--- | :--- | :--- |
| **Telegram Trigger** | Workflow Initiation | Serves as the **entry point**. It activates the workflow whenever a new message is sent to the Telegram bot. |
| **Switch** | Routing / Logic | Acts as a **conditional router**. It directs the workflow down different paths based on specific criteria, like whether the input is a text message or a file. |
| **Telegram (Get a file / Send a message)** | Input / Output | **Handles communication with Telegram**. It's used to download files sent by the user and to send the AI's final response back to the user. |
| **Extract From File (CSV, PDF, etc.)** | Data Extraction | **Parses file content**. A set of specialized nodes, each designed to read and extract data from a specific file type (`.csv`, `.pdf`, `.txt`, etc.). |
| **Code** | Data Transformation | **Standardizes data**. These nodes use JavaScript to transform the extracted data into a consistent format (e.g., converting CSV data into a Markdown table). |
| **Merge** | Flow Control | **Unifies data streams**. It combines the outputs from different extraction paths into a single flow before feeding the data into the RAG pipeline. |
| **Recursive Character Text Splitter** | RAG Pipeline | **Chunks data**. Breaks down the standardized text from documents into smaller, semantically meaningful pieces suitable for embedding. |
| **Embeddings Google Gemini** | RAG Pipeline | **Creates vector embeddings**. Converts the text chunks into numerical representations (vectors) using Google's Gemini model. |
| **Supabase Vector Store** | RAG Pipeline / AI Tool | **Acts as the knowledge base**. It stores the vector embeddings for later retrieval (Insert mode) and allows the AI Agent to search this knowledge base (Tool mode). |
| **AI Agent (Main Orchestrator)** | AI Core | **The central "brain"**. This agent analyzes the user's text query, determines the intent, and delegates the task to the appropriate tool or specialist. |
| **NoSQL\_Specialist / SQL\_Specialist** | AI Core / Tool | **Specialized sub-agents**. These are tools for the main agent. They receive high-level commands and use their own set of tools to perform database operations. |
| **Google Gemini Chat Model** | AI Core | **The Language Model (LLM)**. Provides the reasoning and language understanding capabilities for all the AI agents in the workflow. |
| **Simple Memory** | AI Core | **Maintains context**. Stores the history of the conversation, allowing the AI to remember previous user messages and its own responses. |
| **MongoDB Tools (Find, Insert, etc.)** | Database Operations | A suite of tools used by the **NoSQL Specialist** to perform CRUD (Create, Read, Update, Delete) and aggregation operations in a MongoDB database. |
| **Airtable Tools (Get, Search, etc.)** | Database Operations | A suite of tools used by the **SQL Specialist** to perform CRUD operations on records within a structured Airtable base. |
