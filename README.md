# Blogbot - AI-Powered Blog Knowledge Base

An automated workflow for creating an AI-powered RAG (Retrieval Augmented Generation) agent that can chat with your blog knowledge base using N8N.

## 🚀 Features

- **Automated Blog Content Ingestion**: Fetches all blog URLs from sitemap and scrapes content
- **Vector Database Storage**: Stores blog content in Qdrant for semantic search
- **AI-Powered Chat**: Interactive Q&A using Ollama LLM with RAG capabilities
- **Deduplication**: Intelligent content deduplication to maintain clean vector store
- **Scheduled Updates**: Automated pipeline to keep knowledge base current
- **PostgreSQL Integration**: Robust metadata storage and processing status tracking

## 📋 Prerequisites

- [N8N](https://n8n.io/) - Workflow automation platform
- [PostgreSQL](https://www.postgresql.org/) - Database for URL and metadata storage
- [Qdrant](https://qdrant.tech/) - Vector database for semantic search
- [Ollama](https://ollama.ai/) - Local LLM for embeddings and chat responses

## 🛠️ Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/paresha05/Blogbot.git
   cd Blogbot
   ```

2. **Import N8N Workflow**
   - Open N8N interface
   - Import the `blogbot.json` workflow file
   - Configure your database connections and API credentials

3. **Database Setup**
   - Ensure PostgreSQL is running
   - The workflow will automatically create required tables (`blog_urls` and `blog_content`)

4. **Vector Database Setup**
   - Start Qdrant vector database
   - The workflow will create the `myblog-blogs` collection automatically

5. **Configure Ollama**
   - Install and start Ollama
   - Pull the required model: `ollama pull llama3.2:latest`

## 📁 Project Structure

- `blogbot.json` - N8N workflow configuration
- `blogbot.md` - Detailed workflow documentation
- `Flowchart | Mermaid Chart.png` - Visual workflow diagram
- `Workflow Screenshot.png` - N8N interface screenshot
- `README.md` - This file

## 🔧 Configuration

### Database Tables
The workflow creates and manages two main tables:
- `blog_urls` - Stores blog URLs and processing status
- `blog_content` - Stores scraped content metadata

### Key Components
1. **Sitemap Processing**: Automatically discovers new blog posts
2. **Content Scraping**: Extracts title, content, author, tags, and metadata
3. **Chunking & Embedding**: Splits content and creates vector embeddings
4. **AI Chat Agent**: Provides conversational interface with memory

## 💬 Usage

### Manual Execution
1. Use the Manual Trigger in N8N to run the workflow
2. Monitor processing logs and status updates

### Scheduled Execution  
1. Configure the Schedule Trigger for automated runs
2. Recommended: Daily or weekly updates depending on blog posting frequency

### Chat Interface
1. Use the Chat Trigger to interact with your blog knowledge base
2. Ask questions about your blog content and get AI-powered responses

## 🎯 Key Features Explained

### Data Ingestion Flow
1. **Sitemap** → **Blog URLs** → **Database**
2. **Pending URLs** → **Scraping** → **Content Extraction**
3. **Content** → **Chunking** → **Embedding** → **Qdrant**

### AI Chat Flow
1. **User Query** → **Embedding** → **Qdrant Search** → **Context Retrieval**
2. **Context** → **AI Agent** → **LLM Response** → **User**

### Deduplication Logic
- Checks for existing content by URL before processing
- Removes duplicates from vector store automatically
- Updates database status to track processing state

## 🔍 Customization

- **Metadata Extraction**: Modify CSS selectors for different blog layouts
- **Chunk Size**: Adjust Token Splitter settings for optimal embedding
- **LLM Model**: Change Ollama model (mistral, llama3, etc.)
- **Chat Personality**: Customize AI agent prompts and system messages

## 🐛 Error Handling

- Continues processing on individual failures
- Comprehensive logging for debugging
- Status tracking in database for monitoring
- Duplicate detection prevents vector store pollution

## 📊 Use Cases

- **Knowledge Base Creation**: Build searchable AI-powered blog archives
- **Content Discovery**: Find relevant blog posts through semantic search
- **Customer Support**: Auto-answer questions using blog content
- **Content Analysis**: Understand themes and topics across your blog

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 📞 Support

If you need help or have questions:
- Open an issue on GitHub
- Check the detailed workflow documentation in `blogbot.md`
- Review the visual flowchart for workflow understanding

## 🙏 Acknowledgments

- Built with [N8N](https://n8n.io/) workflow automation
- Powered by [Ollama](https://ollama.ai/) for local LLM capabilities
- Uses [Qdrant](https://qdrant.tech/) for vector storage
- Stores metadata in [PostgreSQL](https://www.postgresql.org/)

---

**Happy Blogging with AI! 🤖📝**
