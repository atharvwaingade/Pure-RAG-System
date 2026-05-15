# Pure RAG System

A lightweight Retrieval-Augmented Generation (RAG) project focused on **retrieval-first answers** without relying on external LLM APIs.

This repository includes:
- A pure RAG indexing/retrieval pipeline (`app/core/*`)
- A production-style FastAPI chat engine (`app/routes/chat.py`)
- Sample ingestion tooling and product data (`data/*`)

## Highlights

- ✅ Retrieval-first architecture
- ✅ No external AI API requirement for the chat engine flow
- ✅ Built-in caching and warmup logic in chat engine
- ✅ Bilingual handling in chat engine (English + Marathi variants)
- ✅ Built-in self-test suite (129 tests)

## Project Structure

```text
app/
  core/            # chunking, embeddings, storage, retrieval
  models/          # request/response schemas
  routes/          # API routes, including main chat engine
    chat.py        # monolithic ERP Intelligence Engine v8.0 + tests

data/
  products.json    # sample dataset
  ingest_file.py   # sample ingestion script

build.sh           # dependency install script
requirements.txt   # Python dependencies
```

## Requirements

- Python 3.11+

Install dependencies:

```bash
bash build.sh
```

> Note: `app/core` modules use `faiss` and `sentence-transformers`; install those separately if you plan to run the pure indexing/retrieval pipeline endpoints.

## Run the API

Start the FastAPI server:

```bash
uvicorn app.routes.chat:app --host 0.0.0.0 --port 8000 --reload
```

Open:
- Swagger UI: `http://localhost:8000/docs`
- Health check: `http://localhost:8000/health`

## Main Endpoints

### `POST /api/chat`
Chat with the retrieval engine.

Example request:

```json
{
  "query": "show today's sales",
  "session_id": "demo-user"
}
```

### `GET /health`
Returns service health, readiness, cache stats, and module status.

### `GET /`
Returns quick service metadata and endpoint pointers.

## Run Tests

The chat engine contains a built-in self-test suite:

```bash
python app/routes/chat.py
```

Expected result: `129/129 passed` (or latest equivalent for the current version).

## Sample Data Ingestion Script

Use the example script to send product records:

```bash
python data/ingest_file.py
```

Before running it, ensure the API is running and review/update constants in `data/ingest_file.py` (API URL, client ID, input file).

## License

This project is licensed under the terms in the [LICENSE](LICENSE) file.
