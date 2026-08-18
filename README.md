# LangChain / LangGraph AI Projects Collection

A collection of practical notebooks demonstrating LangChain, LangGraph, Groq, ChromaDB, RAG, tool-calling agents, conversation memory, and multimodal AI.

## Included Notebooks

| Notebook | Purpose |
|---|---|
| `simple_llm.ipynb` | Basic Groq LLM invocation |
| `vector_db_basic.ipynb` | Chroma vector database and similarity search |
| `rag.ipynb` | PDF-based telecom RAG |
| `single agent.ipynb` | Product/review tool-calling agent |
| `memory.ipynb` | Agent memory with `InMemorySaver` |
| `multimodel.ipynb` | Multimodal image + tool agent |
| `2_blood_work.ipynb` | Blood-report extraction and diet suggestions |
| `telecom_guide.pdf` | Telecom RAG knowledge source |

These are separate learning examples, not one application with a single Python entry point.

---

# Features

- Groq LLM integration
- LangChain chat models
- Chroma vector database
- Semantic similarity search
- PDF document loading
- Recursive document chunking
- Hugging Face embeddings
- Retrieval-Augmented Generation (RAG)
- LangChain agents
- Custom tools
- Tool calling
- Conversation memory
- LangGraph `InMemorySaver`
- Multimodal image input
- Domain-specific AI workflows

---

# Requirements

Recommended:

- Python 3.10+
- Jupyter Notebook or JupyterLab
- Internet connection for model downloads
- Groq API key

---

# 1. Create the Python Environment

From the project directory:

```bash
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

---

# 2. Install Jupyter

```bash
pip install notebook
```

Or:

```bash
pip install jupyterlab
```

---

# 3. Install Dependencies

There is no single requirements file in the supplied notebooks, so install the packages used by the examples:

```bash
pip install python-dotenv
pip install langchain
pip install langchain-core
pip install langchain-community
pip install langchain-text-splitters
pip install langchain-groq
pip install langchain-huggingface
pip install langchain-chroma
pip install chromadb
pip install sentence-transformers
pip install pypdf
pip install langgraph
```

If a notebook reports a missing module, install the corresponding package in the active virtual environment.

---

# 4. Configure Groq

Create `.env` in the project directory:

```env
GROQ_API_KEY=your_groq_api_key_here
```

The notebooks use `load_dotenv()` and Groq models such as:

```text
llama-3.3-70b-versatile
```

The multimodal notebook also uses a vision-capable Groq model.

**Never commit your real API key.**

Recommended `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
.ipynb_checkpoints/
```

---

# 5. Start Jupyter

From the project directory:

```bash
jupyter notebook
```

Or:

```bash
jupyter lab
```

Open the required `.ipynb` file in the browser.

---

# 6. Recommended Run Order

For learning the project from basic to advanced:

```text
simple_llm.ipynb
        ↓
vector_db_basic.ipynb
        ↓
rag.ipynb
        ↓
single agent.ipynb
        ↓
memory.ipynb
        ↓
multimodel.ipynb
        ↓
2_blood_work.ipynb
```

Each notebook can also be run independently after its prerequisites are prepared.

---

# 7. Run `simple_llm.ipynb`

This is the first setup test.

It initializes a Groq chat model and sends:

```text
hello
```

Run every cell from top to bottom.

Expected flow:

```text
Human message
     ↓
Groq LLM
     ↓
AI response
```

If this works, your Python environment and Groq API configuration are working.

---

# 8. Run `vector_db_basic.ipynb`

This demonstrates ChromaDB.

The notebook:

1. Creates a Chroma client.
2. Creates a `news_article` collection.
3. Adds sample documents.
4. Reads stored documents/embeddings.
5. Performs similarity search.

Run all cells from top to bottom.

The query is similar to:

```python
collection.query(
    query_texts=["this is the new battery"],
    n_results=2
)
```

This demonstrates:

```text
Documents
   ↓
Chroma
   ↓
Embeddings
   ↓
Similarity Search
   ↓
Relevant Documents
```

---

# 9. Run `rag.ipynb`

This is the main telecom RAG example.

## RAG pipeline

```text
telecom_guide.pdf
       ↓
PyPDFLoader
       ↓
PDF pages
       ↓
RecursiveCharacterTextSplitter
       ↓
Chunks
       ↓
Hugging Face Embeddings
       ↓
Chroma
       ↓
Retriever
       ↓
Relevant context
       ↓
Groq LLM
       ↓
Answer
```

The supplied telecom guide is a nine-page technical reference covering mobile networks, connectivity troubleshooting, data plans, roaming, SIM technology, VoLTE/VoWiFi, billing, and security. fileciteturn6file0L2-L7

## Important path change

The notebook contains a machine-specific path:

```python
book_path = "D:/langchain/telecom_guide.pdf"
```

Change it to your actual location.

Recommended if the PDF is beside the notebook:

```python
book_path = "./telecom_guide.pdf"
```

Then run the notebook from top to bottom.

## PDF chunking

The notebook uses:

```python
RecursiveCharacterTextSplitter(
    chunk_size=600,
    chunk_overlap=100
)
```

## Embeddings

It uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The first run may download this model.

## Retrieval

The retriever uses:

```python
search_kwargs={"k": 3}
```

so three relevant chunks are retrieved for a question.

## Example

The notebook tests a question about international roaming.

The supplied guide explains roaming, partner networks, roaming zones, activation, bundles, and common failures. fileciteturn6file0L84-L108

---

# 10. Run `single agent.ipynb`

This notebook demonstrates a tool-calling agent.

It defines:

```text
get_product()
get_review()
```

The sample product data includes:

- Wireless headphones
- Smart watch
- Mechanical keyboard
- Laptop stand

Run cells from top to bottom.

Then the example asks:

```text
how do people like smart watch
```

The agent decides whether to call a tool and then generates the final answer.

Flow:

```text
Question
   ↓
Agent
   ↓
Tool selection
   ↓
get_product / get_review
   ↓
Tool result
   ↓
Agent
   ↓
Final answer
```

---

# 11. Run `memory.ipynb`

This extends the agent example with memory.

It uses:

```python
from langgraph.checkpoint.memory import InMemorySaver
```

and:

```python
checkpointer=InMemorySaver()
```

A conversation is identified using a `thread_id`, for example:

```python
config = {
    "configurable": {
        "thread_id": "user-alice-session-1"
    }
}
```

Run:

```text
how do people like smart watch
```

and then:

```text
how do people like the product we discussed before
```

Because both messages use the same thread, the agent can use previous conversation state.

---

# 12. Run `multimodel.ipynb`

This demonstrates multimodal AI.

It uses:

- An image
- Base64 encoding
- A vision-capable Groq model
- Tools
- Agent memory

## Required image

The notebook contains a machine-specific path similar to:

```text
D:/langchain/7_multi_model/blood_work.png
```

Change it to your actual file path.

For example:

```python
with open("./blood_work.png", "rb") as file:
    image_base64 = base64.b64encode(file.read()).decode()
```

The notebook sends the image to the model for blood-report extraction and exposes the extracted report through a tool.

It also defines:

```text
diet_recommendation()
get_blood_report()
```

Flow:

```text
Blood report image
       ↓
Vision model
       ↓
Extracted results
       ↓
Agent
       ↓
Diet recommendation tool
       ↓
Response
```

---

# 13. Run `2_blood_work.ipynb`

This notebook works with a text blood report.

It expects:

```text
bloods_report.txt
```

The notebook opens:

```python
with open("bloods_report.txt", "r") as f:
    blood_work = f.read()
```

Therefore, place `bloods_report.txt` in the notebook's working directory or change the path.

The workflow is:

```text
Blood report text
       ↓
Groq LLM
       ↓
Extract all test values
       ↓
HIGH / LOW / NORMAL
       ↓
Health summary
       ↓
Indian diet suggestions
```

The notebook asks the model to classify each value according to the reference range included in the report.

**Important:** this is an AI demonstration and should not be treated as medical diagnosis or professional medical advice.

---

# 14. Suggested Folder Structure

A clean arrangement is:

```text
project/
│
├── .env
├── .gitignore
│
├── simple_llm.ipynb
├── vector_db_basic.ipynb
├── rag.ipynb
├── single agent.ipynb
├── memory.ipynb
├── multimodel.ipynb
├── 2_blood_work.ipynb
│
├── telecom_guide.pdf
├── blood_work.png
└── bloods_report.txt
```

Only the files required by the notebook you are running need to be present.

---

# 15. Exact Full Run Process

If you want to run and demonstrate the complete collection:

## Terminal

```bash
python -m venv .venv
```

Activate it.

Windows:

```bash
.venv\Scriptsctivate
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Install:

```bash
pip install notebook python-dotenv langchain langchain-core langchain-community langchain-text-splitters langchain-groq langchain-huggingface langchain-chroma chromadb sentence-transformers pypdf langgraph
```

Create `.env`:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Start:

```bash
jupyter notebook
```

Then execute:

```text
1. simple_llm.ipynb
2. vector_db_basic.ipynb
3. rag.ipynb
4. single agent.ipynb
5. memory.ipynb
6. multimodel.ipynb
7. 2_blood_work.ipynb
```

Run each notebook **cell by cell from top to bottom**.

---

# 16. Troubleshooting

## `ModuleNotFoundError`

Make sure the virtual environment is active:

```bash
.venv\Scriptsctivate
```

Then install the missing package.

---

## Groq API key error

Verify `.env`:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Restart the Jupyter kernel after changing environment variables.

---

## PDF not found

Change the hard-coded path in `rag.ipynb`:

```python
book_path = "D:/langchain/telecom_guide.pdf"
```

to:

```python
book_path = "./telecom_guide.pdf"
```

or another valid local path.

---

## Blood image not found

Update the path in `multimodel.ipynb`:

```python
with open("./blood_work.png", "rb") as file:
    ...
```

---

## `bloods_report.txt` not found

Place it beside the notebook or change:

```python
open("bloods_report.txt", "r")
```

to the correct path.

---

## Embedding model download

The RAG notebook uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The first run may download it. This is expected.

---

## Chroma data disappears

The basic vector database example uses:

```python
chromadb.Client()
```

so it is a simple in-memory demonstration rather than a configured production persistent database.

---

# 17. Important Notes

- These notebooks are separate demonstrations.
- Always execute notebook cells from top to bottom unless you understand the dependencies between cells.
- Replace all machine-specific `D:/...` paths.
- Keep `.env` private.
- Keep required input files in accessible paths.
- Restart the Jupyter kernel if environment variables or package installations are changed.
- Some LangChain APIs change between package versions; if an import fails, install compatible/current versions of the corresponding LangChain package.

---

# 18. Security

Add the following to `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
.ipynb_checkpoints/
```

Never publish:

```text
GROQ_API_KEY
```

If an API key is accidentally exposed, rotate/revoke it.

---

# 19. Learning Progression

The project demonstrates a useful progression:

```text
Simple LLM
    ↓
Vector Database
    ↓
RAG
    ↓
Tool-Calling Agent
    ↓
Agent Memory
    ↓
Multimodal Agent
    ↓
Domain-Specific AI
```

The telecom RAG knowledge base is grounded in the supplied reference guide rather than general web knowledge. For example, the guide provides specific connectivity troubleshooting steps including signal checking, airplane-mode reset, APN verification, SIM inspection, outage checking, alternate-device testing, and escalation. fileciteturn6file0L33-L53

---

# License

No license information was provided with the supplied project files.

If you publish the project, add an appropriate `LICENSE` file.
