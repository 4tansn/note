# RAG Application Architecture

## Overview
This document explains the architecture and design of the Retrieval-Augmented Generation (RAG) application built to retrieve context from Confluence pages using embeddings and large language models (LLMs). The system is composed of four primary components: Confluence Scraper, ReactJS Chatbot UI, Backend for UI, and Postgres VectorDB.

## System Components

### 1. Confluence Scraper
- **Function**: Scrapes content from Confluence pages and generates vector embeddings using **bge** (BigGraph Embeddings).
- **Process**:
  1. Extracts data from Confluence.
  2. Transforms text into vector embeddings.
  3. Stores embeddings in the Postgres VectorDB for efficient retrieval.

### 2. Chatbot UI (ReactJS)
- **Function**: A frontend user interface allowing users to input queries and receive responses augmented by relevant context from Confluence pages.
- **Process**:
  1. Takes user input as a query.
  2. Forwards the query to the backend service.
  3. Displays responses from the LLaMA LLM augmented with context.

### 3. Backend for UI
- **Function**: Serves as an intermediary between the ReactJS UI and the other system components (LLM, VectorDB).
- **Process**:
  1. Receives user queries from the frontend.
  2. Queries the Postgres VectorDB for similar embeddings.
  3. Augments the LLaMA model's response with the retrieved context.
  4. Sends the final response back to the UI for display.

### 4. Postgres VectorDB
- **Function**: A vector database that stores the embeddings generated from the Confluence pages for fast similarity search.
- **Process**:
  1. Stores embeddings as vectors.
  2. Returns the most similar embeddings when queried by the backend to provide contextual data.

## Design Diagram

```mermaid
graph TD
    A[Confluence Pages] -->|Scraped Content| B[Confluence Scraper]
    B -->|Generate Embeddings| C[Postgres VectorDB]
    UI[Chatbot UI (ReactJS)] -->|User Query| BE[Backend]
    BE -->|Retrieve Embeddings| C
    BE -->|Call LLama LLM| LLM[LLaMA Model]
    C -->|Return Relevant Context| BE
    BE -->|Augmented Response| UI
