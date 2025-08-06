# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Course Materials RAG (Retrieval-Augmented Generation) system - a full-stack web application that enables users to query course materials and receive intelligent, context-aware responses. The system uses ChromaDB for vector storage, Anthropic's Claude for AI generation, and provides a web interface for interaction.

## Architecture

### Core Components

- **RAG System (`backend/rag_system.py`)**: Main orchestrator that coordinates all components
- **Vector Store (`backend/vector_store.py`)**: ChromaDB-based vector storage for course content and metadata
- **Document Processor (`backend/document_processor.py`)**: Handles parsing and chunking of course documents
- **AI Generator (`backend/ai_generator.py`)**: Interfaces with Anthropic's Claude API using tool-based approach
- **Session Manager (`backend/session_manager.py`)**: Manages conversation history and user sessions
- **Search Tools (`backend/search_tools.py`)**: Tool-based search system for retrieving relevant content

### Data Models (`backend/models.py`)

- **Course**: Represents a complete course with title, instructor, and lessons
- **Lesson**: Individual lesson within a course with number, title, and optional link
- **CourseChunk**: Text chunks for vector storage with course/lesson context

### Frontend Structure

- **Static Files**: Simple HTML/CSS/JS frontend served from `/frontend/`
- **API Integration**: Communicates with FastAPI backend via REST endpoints

## Development Commands

### Environment Setup

```bash
# Install dependencies
uv sync

# Create .env file with required variables
echo "ANTHROPIC_API_KEY=your_api_key_here" > .env
```

### Running the Application

```bash
# Quick start using provided script
chmod +x run.sh
./run.sh

# Manual start
cd backend && uv run uvicorn app:app --reload --port 8000
```

### Key API Endpoints

- `POST /api/query`: Process user queries with RAG system
- `GET /api/courses`: Get course statistics and analytics
- Web Interface: `http://localhost:8000`
- API Docs: `http://localhost:8000/docs`

## Configuration (`backend/config.py`)

Key settings that can be adjusted:
- `CHUNK_SIZE`: 800 characters (text chunk size for vector storage)
- `CHUNK_OVERLAP`: 100 characters (overlap between chunks)
- `MAX_RESULTS`: 5 (maximum search results returned)
- `MAX_HISTORY`: 2 (conversation messages to remember)
- `EMBEDDING_MODEL`: "all-MiniLM-L6-v2" (sentence transformer model)
- `ANTHROPIC_MODEL`: "claude-sonnet-4-20250514"

## Document Processing

The system processes course documents from the `docs/` directory:
- Supports `.pdf`, `.docx`, and `.txt` files
- Documents are automatically chunked and indexed on startup
- Course titles are used as unique identifiers to prevent duplicates

## Tool-Based Search Architecture

The system uses a tool-based approach where Claude can call search functions to retrieve relevant course content:
- `CourseSearchTool`: Semantic search through course materials
- `ToolManager`: Manages available tools and handles tool execution
- Sources are tracked and returned with responses for transparency

## Dependencies

Core dependencies (see `pyproject.toml`):
- `fastapi` + `uvicorn`: Web framework and ASGI server
- `chromadb`: Vector database for embeddings
- `anthropic`: Claude AI API client
- `sentence-transformers`: Text embeddings
- `python-multipart`: File upload support
- `python-dotenv`: Environment variable management

## File Structure Notes

- `main.py`: Basic entry point (not used in web application)
- `run.sh`: Convenience script for starting the application
- `uv.lock`: Dependency lock file managed by uv package manager
- `docs/`: Default location for course materials to be indexed
- `backend/chroma_db/`: ChromaDB storage directory (created at runtime)