
# Documentation Crawler and RAG Agent

An intelligent documentation crawler and RAG (Retrieval-Augmented Generation) agent that transforms documentation websites into an interactive knowledge base. Built with Pydantic AI and Supabase, this system crawls documentation, indexes it in a vector database, and provides AI-powered answers to user queries using contextually relevant documentation chunks.

## Core Capabilities

- Intelligent web crawling with automatic documentation structure detection
- Advanced content chunking with preservation of code blocks and context
- Vector-based semantic search powered by OpenAI embeddings 
- RAG-enhanced question answering with source citations
- Production-ready API endpoints for integration
- Interactive Streamlit web interface for direct usage
- Robust error handling and retry mechanisms

## Technical Requirements

- Python 3.11 or newer
- Supabase account with vector database enabled
- OpenAI API access
- Streamlit (for web interface)
- 2GB+ RAM recommended

## Quick Start

1. Clone and setup the environment:
```bash
git clone https://github.com/coleam00/ottomator-agents.git
cd ottomator-agents/crawl4AI-agent

python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

2. Configure your environment:
```bash
cp .env.example .env
```

Update `.env` with your credentials:
```env
OPENAI_API_KEY=sk-...
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=eyJ...
LLM_MODEL=gpt-4-turbo  # Or your preferred model
CHUNK_SIZE=5000        # Optional: Adjust chunking size
```

3. Initialize the database:
```bash
psql -h database.supabase.co -d postgres -U postgres -f site_pages.sql
```
Or copy the contents of `site_pages.sql` into Supabase's SQL Editor.

4. Start crawling:
```bash
python crawl_pydantic_ai_docs.py --url https://your-docs-site.com
```

5. Launch the UI:
```bash
streamlit run streamlit_ui.py
```

## Architecture

### Database Schema
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT NOT NULL,
    chunk_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    summary TEXT,
    content TEXT NOT NULL,
    metadata JSONB DEFAULT '{}',
    embedding VECTOR(1536),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc', NOW()),
    UNIQUE(url, chunk_number)
);
```

### Key Components

- **Crawler Engine**: Intelligent web crawler with rate limiting and retry logic
- **Content Processor**: Advanced text chunking with context preservation
- **Vector Store**: Supabase pgvector integration for semantic search
- **RAG Agent**: Combines retrieved context with LLM for accurate answers
- **API Layer**: FastAPI endpoints for programmatic access
- **Web Interface**: Streamlit-based UI for interactive usage

## Advanced Configuration

### Crawling Settings
```python
CRAWLER_CONFIG = {
    'chunk_size': 5000,
    'overlap': 500,
    'max_retries': 3,
    'retry_delay': 1,
    'preserve_code_blocks': True,
    'max_tokens_per_chunk': 1000
}
```
![Screenshot 2025-01-23 003818](https://github.com/user-attachments/assets/be2a09ae-2125-49e1-8ce0-764b027a93bc)

![Screenshot 2025-01-23 003408](https://github.com/user-attachments/assets/e576198e-aa54-43a7-bb4d-c7ef5e983a02)

![Screenshot 2025-01-23 003328](https://github.com/user-attachments/assets/d77a7c2d-1e4f-4645-a7c6-5cb7739d625f)

![Screenshot 2025-01-23 003838](https://github.com/user-attachments/assets/eb254219-a3f0-44a0-9d18-c315b7b113f8)

### Custom Error Handling
The system implements comprehensive error handling for:
- Network timeouts and connection failures
- Rate limit management for API calls
- Database connection issues
- Invalid or malformed content
- Token limit exceeded scenarios

## Integration Examples

### API Usage
```python
from crawl4ai import RAGAgent

agent = RAGAgent()
response = await agent.query("How do I implement authentication?")
print(response.answer)
print(response.sources)  # Returns relevant documentation URLs
```

### Streaming Interface
```python
async for chunk in agent.stream_response("Explain routing"):
    print(chunk, end="")
```
