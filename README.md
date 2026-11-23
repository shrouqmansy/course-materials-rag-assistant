# Standard Course Q&A RAG System

A Retrieval-Augmented Generation (RAG) system that answers student questions
using course materials from multiple file types (PDF, DOCX, TXT, CSV).
Answers are generated only from the retrieved content, with source attribution.

## Features
- Knowledge base for 3 courses: CS101 (Python), CS220 (Databases & SQL), CS230 (Machine Learning)
- Text extraction, cleaning and chunking from PDF / DOCX / TXT / CSV
- Embeddings with `BAAI/bge-base-en-v1.5` stored in ChromaDB
- Two-stage retrieval: vector search (12 candidates) + cross-encoder reranking (top 4)
- Answer generation with Groq (`openai/gpt-oss-120b`), grounded in the retrieved context
- Source attribution under each answer
- Course filter and Gradio web interface

## Pipeline
1. Load files from `course_materials/`
2. Extract and clean text
3. Chunk text (700 characters, 25-word overlap)
4. Embed chunks and store them in ChromaDB
5. Retrieve candidates, then rerank with `cross-encoder/ms-marco-MiniLM-L-6-v2`
6. Send the top chunks and the question to the LLM
7. Return the answer with its sources

## Project structure
```
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebook.ipynb        # full pipeline + evaluation + Gradio app
├── app.py                # standalone Gradio app
├── course_materials/     # PDF, DOCX, TXT, CSV files for 3 courses
└── screenshots/
```

## Evaluation
13 test questions covering all three courses, all four file types, and one
out-of-scope question (the system must refuse to answer it).

| Metric | Result |
|---|---|
| Retrieval hit rate | 100% |
| Answer accuracy | XX% |

## How to run

### Option 1: Notebook (Kaggle / Colab / Jupyter)
1. Install dependencies: `pip install -r requirements.txt`
2. Get a free API key from [Groq](https://console.groq.com)
3. Open `notebook.ipynb` and run the cells in order
4. Enter your Groq API key when prompted
5. Open the Gradio link printed in the last cell

### Option 2: Standalone app
```bash
pip install -r requirements.txt
export GROQ_API_KEY="your_key_here"      # Windows PowerShell: $env:GROQ_API_KEY="your_key_here"
python app.py
```
Then open the local URL printed in the terminal (usually http://127.0.0.1:7860).

## Tech stack
Python, ChromaDB, Sentence-Transformers, Groq, Gradio, pypdf, python-docx, pandas

## Screenshots
![App](screenshots/app.png)
