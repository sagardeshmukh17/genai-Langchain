# 🎓 Gen AI Student Assistant Chatbot

## 📌 Overview
This project is a **Student Assistant Chatbot** built using **LangChain, OpenAI, Pinecone, and SQLite**.  
It helps students with:
- Course information (fees, duration, eligibility, syllabus)
- Institute policies (attendance, certification, academic rules)
- Combines **structured data (SQLite)** and **unstructured data (Pinecone RAG)** for accurate answers.

---

## ⚙️ Tech Stack
- **Python** (Backend)
- **Streamlit** (Frontend UI)
- **LangChain** (Prompt + Chain management)
- **OpenAI** (LLM + Embeddings)
- **SQLite** (Structured course database)
- **Pinecone** (Vector DB for unstructured documents)

---

## 🗂️ Project Structure
05-lang-chain-rag-db-project/
│
├── app.py                # Streamlit entry point (UI)
├── chatbot_chain.py       # Main chatbot logic (DB + RAG + LLM)
├── config.py              # Loads API keys and model configs from .env
├── db_service.py          # SQLite DB setup and queries
├── llm_provider.py        # OpenAI LLM initialization
├── prompt.py              # LangChain prompt template
├── rag_service.py         # Pinecone RAG ingestion and retrieval
├── requirements.txt       # Python dependencies
├── documents/             # Folder for .txt files (policies, syllabus, etc.)
└── .env                   # Environment variables (ignored in git)


---

## 🗄️ Databases Used

### 1. **SQLite (db_service.py)**
- Stores structured course data.
- Auto‑creates `student.db` with a `courses` table.
- Demo courses inserted:
  - Python Programming (PY101)
  - Data Science (DS101)
  - Generative AI (AI101)

Functions:
- `initialize_database()` → Creates table + inserts demo data.
- `get_all_courses()` → Fetches all courses.
- `search_courses(keyword)` → Searches courses by keyword.

---

### 2. **Pinecone (rag_service.py)**
- Stores unstructured documents (syllabus, policies).
- Uses **OpenAI embeddings** (`text-embedding-3-small`).
- Index name: `student-chatbot`
- Namespace: `student-documents`

Functions:
- `create_index()` → Creates Pinecone index if not exists.
- `load_documents()` → Reads `.txt` files from `documents/`.
- `ingest_documents()` → Splits text into chunks and stores in Pinecone.
- `retrieve_documents(question)` → Retrieves relevant docs for a query.

---

## 🧠 Prompt + LLM

### **prompt.py**
- Defines chatbot instructions:
  - Use **SQLite DB** for course info.
  - Use **RAG knowledge base** for policies/syllabus.
  - Do not invent info.
  - Answer in student‑friendly language.

### **llm_provider.py**
- Initializes OpenAI LLM (`gpt-5.6-luna`).

---

## 🔗 Chatbot Chain (chatbot_chain.py)
- Combines DB + RAG + LLM:
  1. `get_course_data(question)` → Searches SQLite DB.
  2. `get_rag_context(question)` → Retrieves docs from Pinecone.
  3. Passes both into LangChain prompt.
  4. OpenAI generates final answer.

---

## ⚙️ Configuration (.env)
Example `.env` file:
```env
OPENAI_API_KEY=your_openai_key_here
PINECONE_API_KEY=your_pinecone_key_here
PINECONE_INDEX_NAME=student-chatbot
PINECONE_NAMESPACE=student-documents
OPENAI_CHAT_MODEL=gpt-5.6-luna
OPENAI_EMBEDDING_MODEL=text-embedding-3-small

🚀 How to Run:

1. Setup virtual environment
bash
python -m venv .venv
.venv\Scripts\activate

2. Install dependencies
bash
pip install -r requirements.txt

3. Initialize SQLite DB
bash
python -c "from db_service import initialize_database; initialize_database()"

4. Ingest documents into Pinecone
Run:
bash
python rag_service.py

5. Run Streamlit app
bash
streamlit run app.py

SQLite DB → Structured course info
Pinecone RAG → Unstructured syllabus/policies
LangChain + OpenAI → Combines both sources into student‑friendly answers
