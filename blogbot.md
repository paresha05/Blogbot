# Built  workflow for Data Ingestion & AI-Powered RAG Agent to chat with Blog Knowledge Base using N8N

## Purpose
This workflow automates the process of:
- Fetching all blog URLs from MyBlog's sitemap
- Storing and managing URLs in a PostgreSQL database
- Scraping blog content and metadata
- Chunking, embedding, and storing content in Qdrant vector database
- Enabling AI-powered chat and search over the blog knowledge base using n8n's AI Agent node and Ollama LLM

---

## Main Steps & Nodes

### 1. Triggers
- **Manual Trigger**: Allows manual execution for testing.
- **Schedule Trigger**: Enables scheduled runs to keep the knowledge base updated.
- **Chat Trigger**: Listens for incoming chat messages for AI-powered Q&A.

### 2. Fetch & Parse Sitemap
- **Fetch Sitemap**: Downloads the sitemap XML from MyBlog.
- **Extract Blog URLs from Sitemap**: Parses XML, filters for valid blog URLs, normalizes, and prepares for database insertion.

### 3. Database Operations
- **Create Database Tables**: Ensures `blog_urls` and `blog_content` tables exist with proper schema and indexes.
- **Store URLs in Database**: Inserts new blog URLs, deduplicates using unique constraint.
- **Execute a SQL query**: Selects pending URLs for processing.
- **Insert or update rows in a table**: Upserts blog metadata after scraping.

### 4. Scraping & Content Extraction
- **Fetch Blog Content**: Downloads blog pages in batches.
- **extractedData**: Extracts title, content, author, tags, publish date, and URL from HTML using CSS selectors.

### 5. Deduplication & Update
- **HTTP Request for search duplicates**: Checks Qdrant for existing content by URL.
- **If**: Branches based on duplicate detection.
- **update document**: Prepares IDs for deletion if duplicates found.
- **Delete duplicate points of processing url**: Removes duplicates from Qdrant.
- **insert document**: Prepares new content for insertion.

### 6. Chunking, Embedding, and Vector Storage
- **Token Splitter**: Splits content into chunks for embedding.
- **Default Data Loader**: Loads chunked data and attaches metadata.
- **Embeddings Ollama**: Generates embeddings using Ollama LLM (model: llama3.2:latest).
- **Qdrant Vector Store**: Inserts embedded chunks into Qdrant (`myblog-blogs` collection).

### 7. Verification & Summary
- **get inserted document response**: Verifies successful insert and collects point IDs.
- **update database with fetched urls.**: Updates status and stores Qdrant point IDs in database.
- **Create Processing Summary**: Summarizes scraping and processing results.
- **Convert to File**: Generates a report of processed data.

### 8. AI Chat & Knowledge Base Search
- **AI Agent**: Handles chat-based Q&A using the blog knowledge base.
- **Simple Memory**: Maintains chat context for multi-turn conversations.
- **Ollama Chat Model**: Provides LLM responses using local Ollama (model: llama3.2:latest).
- **Qdrant Vector Store1**: Retrieves relevant context from Qdrant for RAG (Retrieval Augmented Generation).
- **Embeddings Ollama1**: Embeds user queries for semantic search.

---

## Data Flow

1. **Sitemap → Blog URLs → Database**
2. **Pending URLs → Scraping → Content Extraction**
3. **Content → Chunking → Embedding → Qdrant**
4. **Deduplication → Update Database**
5. **Chat Query → Embedding → Qdrant Search → Context → AI Agent → Response**

---

## Key Integrations

- **PostgreSQL**: Stores URLs and metadata, tracks processing status.
- **Qdrant**: Stores vectorized blog content for semantic search and RAG.
- **Ollama**: Local LLM for embeddings and chat responses.
- **n8n AI Agent**: Orchestrates chat, memory, and RAG workflow.

---

## Error Handling

- Deduplication logic ensures no duplicate vectors in Qdrant.
- On error, nodes are set to continue and output error data for debugging.
- Status fields in database track progress and failures.

---

## Customization Points

- **Metadata Extraction**: CSS selectors can be adjusted for different blog layouts.
- **Chunk Size**: Token Splitter chunk size can be tuned for optimal embedding.
- **LLM Model**: Ollama model can be changed (e.g., mistral, llama3, etc.).
- **Chat Agent Prompt**: Tool description and system message can be customized for different assistant personalities.

---

## Usage Scenarios

- **Automated Blog Knowledge Base Creation**
- **AI-powered Blog Search and Q&A**
- **Continuous Knowledge Base Updates**
- **Deduplication and Data Hygiene for Vector Store**

---

## Summary

This workflow is a robust, production-ready pipeline for building and maintaining an AI-powered blog knowledge base using n8n, PostgreSQL, Qdrant, and Ollama. It supports both automated and chat-driven interactions, with full error handling and deduplication.

If you need a visual diagram or want a breakdown of specific node logic, let me know!
