# Altium n8n Design Reviewer

An intelligent n8n workflow for document processing and AI-powered question answering using Retrieval Augmented Generation (RAG).

## Overview

This repository contains an n8n workflow that implements an agentic RAG system capable of:
- Processing multiple document formats (PDF, Google Docs, Excel, CSV)
- Intelligent document analysis with semantic search
- SQL queries for precise tabular data analysis
- Session-based chat interface

## Quick Start

1. Import the workflow file `oxos_altium_desgin_reviewer.json` into your n8n instance
2. Configure the required credentials:
   - Google Drive OAuth2 API
   - Supabase API
   - PostgreSQL connection
   - Google Gemini API
3. Run the database setup nodes to create required tables
4. Configure Google Drive folder monitoring
5. Start asking questions about your documents!

## Features

- **Multi-format Support**: Handles text documents and spreadsheets
- **Intelligent Routing**: Automatically chooses the best analysis approach
- **Vector Search**: Semantic document search using embeddings
- **Tabular Analysis**: SQL queries for spreadsheet data
- **Chat Memory**: Maintains conversation context
- **Real-time Processing**: Automatic document ingestion from Google Drive

## Architecture

The workflow uses Supabase for vector storage, PostgreSQL for structured data, and Google Gemini for AI processing. Documents are automatically processed and stored in multiple formats to enable both semantic search and precise data analysis.