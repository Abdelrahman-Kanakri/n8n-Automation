#  Restaurant Chatbot (RAG)
<img width="1515" height="498" alt="image" src="https://github.com/user-attachments/assets/314a62ec-baf8-4840-bb82-ec1b4e9a8cef" />


## 1. Overview
| Attribute       | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| **Workflow Name** | Abdelrahman Kanakri Exam Proj 2 (Rag)                               |
| **Purpose**       | Implements a Retrieval-Augmented Generation (RAG) system where an AI Agent answers user questions about the restaurant using documents stored in a dedicated Google Drive folder. |
| **Triggers**      | 1. Google Drive Trigger (for knowledge base updates) <br> 2. Chat Message Received (for user questions) |
| **Dependencies**  | Google Drive, Google Gemini Chat Model, Supabase Vector Store, OpenAI (for embeddings) |
| **Version ID**    | f957099b… (internal)                                                 |

---

## 2. Part 1: Knowledge Base Indexing (The RAG Pipeline)
Automatically indexes new documents from the Google Drive folder into the vector database.

### 2.1. Node Configuration Summary (Indexing Path)
| Node Name           | Type             | Description                                                      |
|--------------------|-----------------|------------------------------------------------------------------|
| **Google Drive Trigger** | Google Drive Trigger | Watches the "Restaurnt Rag System" folder for new files.        |
| **Download file**       | Google Drive       | Downloads the newly created file from Drive.                   |
| **Text Splitter**       | Text Splitter      | Breaks document text into 500-character chunks.               |
| **Embeddings OpenAI**   | Embeddings         | Converts text chunks into numerical vectors using OpenAI.      |
| **Supabase Vector Store** | Vector Store     | Writes chunks and vectors into the Supabase database.          |

### 2.2. Indexing Details
| Service              | Parameter       | Configuration          | Notes                                         |
|---------------------|----------------|----------------------|-----------------------------------------------|
| Google Drive Trigger | Folder Name    | Restaurnt Rag System | Holds all restaurant knowledge documents.    |
| Text Splitter        | Chunks Size    | 500                  | Split size for optimal retrieval.            |
| Embeddings           | Model          | text-embedding-3-small | OpenAI small embedding model.               |
| Supabase             | Table Name     | documents            | Target table in Supabase.                     |

---

## 3. Part 2: User Question & Retrieval (The Q&A Flow)
Handles incoming user questions and generates answers using the indexed vector store.

### 3.1. Node Configuration Summary (Q&A Path)
| Node Name                   | Type                      | Description                                                      |
|------------------------------|--------------------------|------------------------------------------------------------------|
| **When chat message received** | Chat Trigger            | Starts the workflow when a user sends a question (Telegram/Webhook). |
| **Google Gemini Chat Model** | Chat Model               | Provides the language model for the AI Agent.                   |
| **Simple Memory**           | Memory Buffer Window     | Maintains chat history for follow-up questions.                 |
| **AI Agent**                | AI Agent                 | Core RAG node. Retrieves relevant chunks and synthesizes answer. |
| **Tool: Supabase Vector Store1** | Vector Store Tool    | Performs semantic search/retrieval from Supabase database.       |
| **Send a text message**     | Telegram                 | Sends the final answer back to the user.                         |

### 3.2. Agent Logic and Retrieval
The AI Agent is strictly configured to answer questions **only** based on the context retrieved from the Supabase Vector Store1 tool. It does not rely on general knowledge.

---

## 4. Deployment and Setup Notes
- **AI Model:** Uses Gemini 2.5 Pro chat model for generation and synthesis.  
- **External Services:** Requires active accounts/configuration for all services listed.

### Required Credentials
| Service                  | Credential Name/Type       | Placeholder           |
|--------------------------|----------------------------|---------------------|
| Google Drive             | OAuth2 / Service Account   | `YOUR_CREDENTIALS`  |
| Google Gemini (PaLM)     | AI Model API Key           | `YOUR_CREDENTIALS`  |
| OpenAI                   | API Key (for embeddings)   | `YOUR_CREDENTIALS`  |
| Supabase                 | API Key / URL              | `YOUR_CREDENTIALS`  |
| Chat Trigger             | Webhook or Telegram API    | `YOUR_CREDENTIALS`  |
