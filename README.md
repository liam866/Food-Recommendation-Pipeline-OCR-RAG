# Menu-to-Insight: OCR & RAG Food Recommendation Pipeline

Menu-to-Insight is a containerised AI pipeline that transforms restaurant menu images into structured dietary recommendations. Menu text is extracted via OCR, embedded in a vector database, and retrieved as context for a local LLM to generate grounded and verifiable food insights.

The system is built with a decoupled architecture, orchestrated via Docker Compose, with separate services for API, Vision, and Vector processing. Structured logging and observability ensure reliable communication and traceable data flow across services.

---

## Key Features

### Menu Analysis (OCR + RAG)

Users can upload a menu image to receive structured menu items along with goal-aware dietary recommendations.

**Image Processing (Vision Service)**

*   Image preprocessing
*   Layout detection to segment menu regions
*   OCR extraction using PaddleOCR and OpenCV

**Retrieval‑Augmented Generation**

*   Menu items embedded and stored in Qdrant for semantic search
*   A similarity threshold ensures only high‑confidence matches are used
*  Relevant nutritional data retrieved and passed as context to the LLM

**LLM‑Based Recommendations**

*   A local Ollama LLM generates:

    *   Top 3 menu recommendations
    *   Goal‑aware suggestions based on remaining calories and protein
*   The model is strictly constrained to retrieved nutritional context (no hallucinations)

**Explainability**

*   Each suggestion includes nutritional reasoning and data to ensure transparency

---

### Food Logging

Independent food-tracking functionality includes:

*   Manual logging with gram-based quantities
*   Automatic calculation of calories and macronutrients
*   Full CRUD support for entries and daily summaries
*   Comparison against user-defined dietary goals

---

## Architecture Overview

The pipeline is structured into isolated, containerised services:


*   API Service – Orchestrates requests, validates input, and aggregates results.
*   Vision Service – Handles OCR and menu layout detection.
*   Vector Service – Manages embeddings and semantic search using Qdrant.
*   Relational Service – Stores food catalog, nutritional data, and user logs (SQLite + SQLAlchemy).
*   LLM Integration – Local Ollama model generates grounded insights from retrieved data.
  
Data flows cleanly from image ingestion → OCR → vector embedding → LLM → API response.

---

## Tech Stack 

Python, FastAPI, Docker, Ollama, Qdrant, PaddleOCR, OpenCV, SQLAlchemy, Pandas

---

## Running Locally

### Prerequisites

*   **Docker** — Ensure Docker is installed and running
*   **Ollama** — Install locally and pull the required models:

```bash
    ollama pull qwen2.5:0.5b
    ollama pull all-minilm
```

### Start Services

```bash
    docker compose up --build
```

### Access

Open your browser at http://localhost:8000

---

## Possible Extensions

*   Authentication and multi‑user support
*   Long‑term dietary analytics and trend visualization
*   Conversational meal planning
*   Faster, real‑time food logging workflows
