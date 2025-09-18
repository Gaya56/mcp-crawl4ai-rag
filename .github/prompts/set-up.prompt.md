---
mode: agent
---
Define the task to achieve, including specific requirements, constraints, and success criteria.

### Instructions

# Working with MCP-Crawl4AI-RAG:
- Use all available MCP tools (terminal, filesystem, web browsing, memory, brave search, supabase, python execution)
- Move one step at a time and verify each step before proceeding
- For every step, reference `/workspaces/mcp-crawl4ai-rag/README.md` for guidance
- If an error occurs, understand the issue before attempting a solution
- If a step fails, revert changes and try again with a corrected approach
- Validate your work at each milestone before continuing to the next step
- If stuck, first check the repository documentation, then search for solutions from official sources
- for this test we will not be using docker we will be using uv to create a virtual environment. Docker is more for production and uv is more for development.

# Setup Prompt – Crawl4AI RAG MCP Server

You are a GitHub Copilot agent.

Your task: configure and run the **mcp-crawl4ai-rag** server (from https://github.com/coleam00/mcp-crawl4ai-rag) inside GitHub Codespaces.

Goal: crawl a few Alberta government URLs, store them in Supabase, and test RAG queries.

- --

## 1. Required Inputs

Ask the user for:

- **SUPABASE_URL** → found in Supabase dashboard.
- **SUPABASE_SERVICE_KEY** → service_role key from Supabase API settings.
- **OPENAI_API_KEY** → for embeddings and queries.
- *(Optional)* Neo4j creds → only if `USE_KNOWLEDGE_GRAPH=true`.
- --

## 2. `.env` file (root directory)

Reference: [README.md – Environment Setup](https://github.com/coleam00/mcp-crawl4ai-rag/blob/main/README.md)

Create `.env`:

HOST=0.0.0.0

PORT=8051

TRANSPORT=sse

OPENAI_API_KEY=<user_api_key>

MODEL_CHOICE=gpt-4.1-nano

USE_CONTEXTUAL_EMBEDDINGS=false

USE_HYBRID_SEARCH=false

USE_AGENTIC_RAG=false

USE_RERANKING=false

USE_KNOWLEDGE_GRAPH=false

SUPABASE_URL=<user_supabase_url>

SUPABASE_SERVICE_KEY=<user_supabase_service_key>

- --

## 3. Supabase Database

Reference: [`crawled_pages.sql`](https://github.com/coleam00/mcp-crawl4ai-rag/blob/main/crawled_pages.sql)

- Enable **pgvector** extension in Supabase.
- Apply `crawled_pages.sql` via SQL editor.
- This creates the `crawled_pages` table with:

- `id` (uuid, PK)

- `url` (text)

- `content` (text)

- `embedding` (vector)

- --

## 4. Install & Setup (Codespaces terminal)

Reference: [README.md – Install](https://github.com/coleam00/mcp-crawl4ai-rag/blob/main/README.md)

pip install uv

uv venv && source .venv/bin/activate

uv pip install -e .

crawl4ai-setup

- --

## 5. Run MCP Server

Reference: [`src/crawl4ai_mcp.py`](https://github.com/coleam00/mcp-crawl4ai-rag/blob/main/src/crawl4ai_mcp.py)

python src/crawl4ai_mcp.py

- --

## 6. Test Crawl & Query

Tools: `smart_crawl_url`, `perform_rag_query`
- They’re exposed in the server code (src/crawl4ai_mcp.py) and documented in the README under the “Test Crawl & Query” section. So you don’t have to add them yourself — just configure .env, start the server, and those tools are available for use.
- Crawl Alberta URLs:

- https://www.aer.ca/

- https://www.alberta.ca/mineral-ownership

- https://content2.energy.alberta.ca/petroleum-and-natural-gas-tenure-public-offerings-and-results


- Run query:

*“Who regulates mineral ownership and how are land sales handled in Alberta?”*

- --

## Success Criteria

- `.env` is loaded.
- Supabase `crawled_pages` table populated with Alberta URLs.
- RAG query returns a relevant answer.