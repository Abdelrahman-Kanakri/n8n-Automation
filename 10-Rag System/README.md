# Rag System

## Applying Embeddings (Encoding)
<img width="639" height="306" alt="image" src="https://github.com/user-attachments/assets/416dee54-1472-4fd7-87d9-9024f5e51944" />


## Information Retrieval (Decoding)
<img width="508" height="274" alt="image" src="https://github.com/user-attachments/assets/41ffd92b-8215-47e7-a320-2a8f93671783" />

## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Rag System                                                          |
| **Purpose**       | Creates a Retrieval-Augmented Generation (RAG) system where an AI Agent answers user questions by searching and citing documents stored in a specific Google Drive folder. |
| **Triggers**      | 1. Google Drive Trigger (knowledge base updates) <br> 2. Telegram Trigger (user questions) |
| **Dependencies**  | Google Drive, Google Gemini Chat Model, Supabase Vector Store, OpenAI (for embeddings), Telegram |
| **Version ID**    | f6a9829f-7608-466d-974a-251c68f6b4d3                                |

---

## 2. Part 1: Knowledge Base Indexing (The RAG Pipeline)
This path ensures that documents added to the designated Google Drive folder are automatically converted and indexed into the vector database.

### 2.1. Node Configuration Summary (Indexing Path)
| Node Name               | Type                  | Description                                                                 |
|-------------------------|----------------------|-----------------------------------------------------------------------------|
| **Google Drive Trigger** | Google Drive Trigger | Primary Trigger (Indexing). Watches the "Rag" folder for new file creation events. |
| **Extract from File**   | Google Drive         | Reads and extracts text content from the newly uploaded file.              |
| **Text Splitter**       | Text Splitter        | Breaks large document text into smaller chunks for processing.            |
| **Embeddings OpenAI**   | Embeddings           | Converts each text chunk into a dense vector using OpenAI's embedding model. |
| **Supabase Vector Store** | Vector Store        | Writes text chunks and vectors to Supabase database for efficient retrieval. |

### 2.2. Indexing Details
| Service           | Parameter    | Configuration           | Notes                                       |
|------------------|-------------|-------------------------|--------------------------------------------|
| Google Drive Trigger | Folder ID | 1kBOh6obE7hEUbF3yKw6riwRmER-g4Plz | Folder named "Rag" for knowledge base files. |
| Text Splitter      | Chunks Size | 500                     | Documents are split into chunks of 500 characters. |
| Embeddings         | Model       | text-embedding-3-small  | OpenAI model for vector creation.          |
| Supabase           | Table Name  | documents               | Target table in the Supabase database.    |

---

## 3. Part 2: User Question & Retrieval (The Q&A Flow)
This path handles incoming user questions from Telegram and uses the indexed knowledge base to formulate answers.

### 3.1. Node Configuration Summary (Q&A Path)
| Node Name                  | Type                  | Description                                                                 |
|----------------------------|----------------------|-----------------------------------------------------------------------------|
| **When chat message received** | Telegram Trigger     | Primary Trigger (Q&A). Starts workflow when a user sends a question.       |
| **AI Agent**               | AI Agent             | Receives the question, retrieves relevant context from the vector store, and generates a final answer. |
| **Simple Memory**          | Memory Buffer Window | Maintains chat history for contextual follow-up questions.                  |
| **Send a text message**    | Telegram             | Sends AI Agent's answer back to the user.                                   |
| **Tool: Supabase Vector Store1** | Vector Store Tool | Used by the AI Agent to search/lookup relevant documents from the database. |

### 3.2. Agent Logic and Retrieval
| Component          | Purpose                                | Notes                                                                 |
|-------------------|----------------------------------------|----------------------------------------------------------------------|
| **AI Agent**       | Retrieval & Generation                 | System message directs it to answer using only context from Supabase Vector Store1. |
| **Vector Store Tool** | Retrieval Source                     | Uses OpenAI Embeddings model (Embeddings OpenAI1) for search and Supabase Vector Store for lookup. |

---

## 4. Deployment and Setup Notes

- **AI Model:** Gemini 2.5 Pro chat model.  
- **External Services:** Requires configuration of all services below.  

### Required Credentials
| Service             | Credential Name/Type         | Placeholder          |
|--------------------|-----------------------------|--------------------|
| Google Drive        | OAuth2 / Service Account    | `YOUR_CREDENTIALS` |
| Google Gemini (PaLM)| AI Model API Key            | `YOUR_CREDENTIALS` |
| OpenAI              | API Key (for embeddings)    | `YOUR_CREDENTIALS` |
| Supabase            | API Key / URL               | `YOUR_CREDENTIALS` |
| Telegram            | API Key                     | `YOUR_CREDENTIALS` |
