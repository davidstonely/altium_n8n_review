# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains an **n8n Agentic RAG (Retrieval Augmented Generation) workflow** designed for intelligent document processing and AI-powered question answering. The system goes beyond standard RAG by implementing agentic capabilities that can reason about documents, perform SQL queries on tabular data, and dynamically choose the best approach for each question.

## Architecture

The workflow is contained in a single n8n workflow file: `oxos_altium_desgin_reviewer.json`

### Core Components

1. **Document Ingestion Pipeline**
   - Google Drive integration for automatic file monitoring (created/updated triggers)
   - Multi-format support: PDFs, Google Docs, Excel/CSV files, plain text
   - Automatic file type detection and routing through Switch node

2. **Data Processing & Storage**
   - **Vector Storage**: Supabase with pgvector extension for document embeddings
   - **Structured Storage**: PostgreSQL tables for metadata and tabular data
   - **Text Chunking**: Character-based text splitter for optimal RAG performance

3. **Database Schema**
   - `documents` table: Vector embeddings and document chunks with metadata
   - `document_metadata` table: File information, titles, URLs, and schemas
   - `document_rows` table: JSONB storage for tabular data from spreadsheets

4. **AI Agent System**
   - **Language Model**: Google Gemini integration
   - **Embeddings**: Google Gemini embeddings for semantic search
   - **Memory**: PostgreSQL-based chat memory for conversation context
   - **Tools**: RAG search, SQL queries, document retrieval, metadata lookup

5. **Chat Interface**
   - Webhook-based chat trigger for external integrations
   - Session-based memory management
   - Real-time response handling

### Intelligent Tool Selection

The agent automatically chooses between different approaches:
- **RAG Search**: For general document queries and semantic search
- **SQL Queries**: For precise numerical analysis on spreadsheet data
- **Full Document Retrieval**: When complete context is needed
- **Metadata Lookup**: For understanding available documents and schemas

## Key Features

- **Multi-format Processing**: Handles text documents (PDF, Docs) and tabular data (Excel, CSV)
- **Automatic Chunking**: Text documents are split for optimal vector storage
- **JSONB Storage**: Efficient storage of tabular data without creating new tables per file
- **File Versioning**: Automatic cleanup of old document versions on updates
- **Cross-document Analysis**: Can connect information across multiple documents
- **Session Management**: Maintains conversation context across interactions

## Dependencies

The workflow requires the following n8n node packages:
- `@n8n/n8n-nodes-langchain` (AI/ML nodes)
- Standard n8n nodes for Google Drive, Supabase, PostgreSQL

## External Services Required

- **Google Drive**: For document storage and monitoring
- **Supabase**: Vector database with pgvector extension
- **PostgreSQL**: Structured data storage
- **Google Gemini API**: Language model and embeddings

## Setup Requirements

1. **Database Setup**: Run the table creation nodes first to initialize:
   - `Create Documents Table and Match Function`
   - `Create Document Metadata Table`
   - `Create Document Rows Table (for Tabular Data)`

2. **Credentials Configuration**:
   - Google Drive OAuth2 API
   - Supabase API credentials
   - PostgreSQL connection
   - Google Gemini (PaLM) API

3. **Google Drive Configuration**: Set up folder monitoring for document ingestion

## System Prompt Context

The agent uses a comprehensive system prompt that instructs it to:
- Start with RAG unless tabular analysis is needed
- Fall back to full document analysis if RAG is insufficient
- Use SQL for reliable numerical calculations
- Always inform users when answers aren't found

## Workflow Execution

The system operates in two main modes:
1. **Document Processing**: Triggered by Google Drive file changes
2. **Question Answering**: Triggered by chat webhook calls

Document processing includes automatic cleanup of old versions, format-specific extraction, and both vector and structured storage.