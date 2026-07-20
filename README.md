# MediGuide-AI

MediGuide-AI is an AI-powered medical assistant built with Python and Flask. It uses a Retrieval-Augmented Generation (RAG) pipeline to answer medical questions by searching through uploaded PDF documents and generating responses with Google Gemini.

## Project Overview

This project allows users to ask questions in natural language and receive answers grounded in medical documents stored in a vector database. The system combines:

- document ingestion from PDF files
- text chunking and embedding generation
- vector search with Pinecone
- answer generation using Google Gemini

This makes the chatbot more reliable for domain-specific questions because it retrieves relevant context before answering.

## Features

- Conversational web interface built with Flask
- PDF-based knowledge retrieval
- Semantic search using embeddings
- RAG-based answer generation with Gemini
- Easy setup with Python and Docker
- Environment-based configuration using .env

## Technologies Used

- Python 3.10+
- Flask
- LangChain
- LangChain Google GenAI
- LangChain Pinecone
- Sentence Transformers
- PyPDF
- Pinecone Vector Database
- Google Gemini 2.5 Flash
- python-dotenv
- Docker

## Project Structure

```text
MediGuide-AI/
├── app.py                  # Flask app and RAG pipeline
├── store_index.py          # Creates and stores embeddings in Pinecone
├── src/
│   ├── helper.py           # PDF loading, chunking, embeddings
│   └── prompt.py           # System prompt for the assistant
├── Templates/
│   └── chat.html           # Frontend chat UI
├── static/
│   └── style.css           # Styles for the UI
├── data/                   # PDF documents used for retrieval
├── requirements.txt        # Python dependencies
├── Dockerfile              # Docker image configuration
└── README.md               # Project documentation
```

## Prerequisites

Before running the project, make sure you have:

- Python 3.10 or higher installed
- A Pinecone account and API key
- A Google AI Studio account and Gemini API key
- Internet access to download Hugging Face models and connect to APIs

## Installation

1. Clone the repository

```bash
git clone https://github.com/your-username/MediGuide-AI.git
cd MediGuide-AI
```

2. Create and activate a virtual environment

On Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

On Linux / macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Install dependencies

```bash
pip install -r requirements.txt
```

## Create the .env File

Create a file named .env in the project root and add your API keys.

Example content:

```env
PINECONE_API_KEY=your_pinecone_api_key_here
GOOGLE_API_KEY=your_google_gemini_api_key_here
```

### Where to get the keys

- Pinecone API key: from your Pinecone console
- Google API key: from Google AI Studio / Gemini API page

> Keep your .env file private and do not share it publicly.

## Prepare the Knowledge Base

Place your PDF files inside the data/ folder.

Then create the vector index and upload the documents:

```bash
python store_index.py
```

This script will:

- load PDF files from the data directory
- split them into chunks
- generate embeddings
- create or use a Pinecone index named medical-chatbot

## Run the Application

Start the Flask app:

```bash
python app.py
```

Then open your browser and visit:

```text
http://localhost:8080
```

## How the App Works

1. The user enters a question through the web interface.
2. The app sends the question to the RAG chain.
3. Relevant chunks are retrieved from Pinecone.
4. Google Gemini generates a concise and helpful response using that retrieved context.

## API Endpoints

- GET / : Home page
- POST /get : Chat endpoint that accepts form data with msg
- POST /chat : JSON-based chat endpoint that accepts {"message": "..."}

## Docker Usage

You can also run the app using Docker.

Build the image:

```bash
docker build -t mediguide-ai .
```

Run the container:

```bash
docker run -p 8080:8080 --env-file .env mediguide-ai
```

## Notes

- The app expects a Pinecone index to already exist. If it does not, the indexing script will create it.
- If you want to update the knowledge base, replace or add PDFs in the data/ folder and run store_index.py again.
- Make sure your API keys are valid and have the required permissions.

## License

This project is licensed under the MIT License.
