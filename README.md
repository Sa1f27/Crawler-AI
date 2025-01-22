# Documentation Crawler and RAG Agent

An intelligent system for crawling documentation sites and building a smart question-answering interface using Pydantic AI and Supabase. Combines web crawling, vector search, and AI to deliver accurate answers from documentation.

## Features
- Smart documentation crawler
- Vector search with Supabase
- OpenAI embeddings for search
- Context-aware answers
- Code snippet preservation  
- Web interface
- API endpoints

## Setup Requirements
- Python 3.11+
- Supabase database
- OpenAI API access
- Streamlit 

## Installation
1. Get the code:
```bash
git clone https://github.com/coleam00/ottomator-agents.git
cd ottomator-agents/crawl4AI-agent
```

2. Install packages:
```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. Configure settings:
   - Copy `.env.example` to `.env`
   - Add your credentials:
   ```env
   OPENAI_API_KEY=your_key
   SUPABASE_URL=your_url
   SUPABASE_SERVICE_KEY=your_key
   LLM_MODEL=gpt-4-turbo
   ```

## Setup Steps

### Database Init
Run `site_pages.sql` to create:
1. Required tables
2. Vector search
3. Security rules

Access SQL Editor in Supabase to run the commands.

### Start Crawling 
Index documentation:
```bash
python crawl_pydantic_ai_docs.py
```

This will:
1. Get sitemap URLs
2. Process pages
3. Store embeddings

### Launch Interface
Start web UI:
```bash
streamlit run streamlit_ui.py
```

Access at `localhost:8501`

## Settings

### Database Structure
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT,
    chunk_number INTEGER,
    title TEXT,
    summary TEXT,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(1536)
);
```

### Chunk Settings
Adjust in `crawl_pydantic_ai_docs.py`:
```python
chunk_size = 5000  # Characters per chunk
```

Preserves:
- Code blocks
- Paragraphs
- Sentences

## Code Structure
- `crawl_pydantic_ai_docs.py`: Crawler
- `pydantic_ai_expert.py`: QA logic
- `streamlit_ui.py`: Interface
- `site_pages.sql`: Database setup
- `requirements.txt`: Dependencies

## Production Version
Check `studio-integration-api` for the production API implementation.


![Screenshot 2025-01-23 003818](https://github.com/user-attachments/assets/be2a09ae-2125-49e1-8ce0-764b027a93bc)

![Screenshot 2025-01-23 003408](https://github.com/user-attachments/assets/e576198e-aa54-43a7-bb4d-c7ef5e983a02)

![Screenshot 2025-01-23 003328](https://github.com/user-attachments/assets/d77a7c2d-1e4f-4645-a7c6-5cb7739d625f)

![Screenshot 2025-01-23 003838](https://github.com/user-attachments/assets/eb254219-a3f0-44a0-9d18-c315b7b113f8)

## Error Management
Handles:
- Network issues
- API limits
- Database errors
- Embedding fails
- Bad URLs
