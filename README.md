# Agenteer

![Agenteer UI](images/icon.jpeg)

Agenteer is a streamlined AI agent builder focused on simplicity, performance, and usability. It enables the rapid creation, testing, and deployment of AI agents with minimal configuration. Agenteer produces functioal agents using Pydantic.ai, LangChain, Anthropic MCP and OpenAI modules

## Key Features

- **Zero-Config Setup**: Get started with minimal setup requirements
- **Unified Interface**: CLI and web UI in a single package
- **Local First**: Runs entirely on local resources when needed
- **Extensible**: Plugin architecture for custom agent capabilities
- **Performance**: Optimized for speed with local caching
- **Documentation Crawler**: Automatically index documentation with progress tracking and caching
- **GitHub Integration**: Create GitHub-specific agents with repository access

## Installation

### Local Installation

```bash
# Clone the repository
git clone https://github.com/ckoons/Agenteer.git
cd Agenteer

# Run the setup script
./setup.sh

# Activate the virtual environment
source venv/bin/activate

# Start the UI
agenteer ui

# Or use the wrapper script for cleaner output
./run_ui.sh

# Or use the CLI
agenteer create -n "my_agent" -d "A weather agent that fetches forecast data"
```

### Docker Installation

```bash
# Build and run with Docker Compose
docker-compose up -d

# Or build and run manually
docker build --platform linux/arm64 -t agenteer .  # For Apple Silicon Macs
docker build --platform linux/amd64 -t agenteer .  # For Intel/AMD systems
docker run -p 8501:8501 -p 8000:8000 agenteer
```

> **Note for Apple Silicon Mac users**: Be sure to include the `--platform linux/arm64` flag when building and running Docker images to ensure compatibility.

### Documentation Preloading

Agenteer offers built-in documentation preloading to enhance agent capabilities. There are several ways to preload documentation:

1. **Via the UI (Recommended)**: 
   Navigate to the Documentation or Web Search pages in the UI. If no documentation is found, you'll see a "Preload Essential Documentation" button that will automatically crawl and index documentation from Pydantic, LangChain, and Anthropic.

2. **Using the CLI**:
   ```bash
   # Preload all documentation sources
   agenteer preload-docs

   # Preload a specific source
   agenteer preload-docs --source pydantic
   agenteer preload-docs --source langchain
   agenteer preload-docs --source anthropic

   # Customize crawling settings
   agenteer preload-docs --max-pages 500 --max-depth 4 --timeout 300
   ```

3. **With Docker**:
   ```bash
   # Preload all documentation sources
   docker run --platform linux/arm64 agenteer preload-docs  # For Apple Silicon Macs
   docker run --platform linux/amd64 agenteer preload-docs  # For Intel/AMD systems

   # Preload a specific source
   docker run --platform linux/arm64 agenteer preload-docs --source langchain  # For Apple Silicon
   ```

The preloaded documentation enables agents to leverage knowledge from these frameworks when responding to queries, making them more effective for framework-specific tasks.

Agenteer automatically caches documentation for faster preloading in future sessions. Cached documentation is stored in the `vector_store/doc_cache` directory and is valid for 7 days, after which it will be refreshed automatically. This ensures your documentation stays current while minimizing network usage.

## Requirements

- Python 3.10+
- No external services required (uses SQLite by default)
- LLM API key (supports Claude, OpenAI, or local models via Ollama)

## UI Overview

The Agenteer UI is designed to be intuitive and streamlined:

- **Home**: Dashboard with quick actions and recent activity
- **Create Agent**: Form to create new agents with different capabilities
- **Existing Agents**: Browse and interact with your created agents
- **Documentation**: Search, crawl, and browse documentation
- **Web Search**: Crawl and index external documentation
- **Settings**: Configure API keys, database, and models

## Architecture

Agenteer consists of several core components:

1. **Core Engine**: Agent generation and execution logic
2. **Local Database**: SQLite-based storage for agents and sessions
3. **Vector Store**: FAISS-based document embedding and retrieval
4. **UI**: Streamlit-based web interface 
5. **CLI**: Command-line interface for automation and scripting
6. **API**: REST API for integration with other tools
7. **Documentation Crawler**: Tools for indexing documentation

## Configuration

Agenteer uses a hierarchical configuration system:

- `.env`: Base configuration shared by all users
- `.env.owner`: Personal settings (API keys, model preferences)
- `.env.local`: Local development settings

Example configuration:

```bash
# API Keys
OPENAI_API_KEY="your-openai-key"
ANTHROPIC_API_KEY="your-anthropic-key"
OLLAMA_BASE_URL="http://localhost:11434"

# Model settings
DEFAULT_MODEL="claude-3-7-sonnet-20250219"
USE_LOCAL_MODELS=false
EMBEDDING_MODEL="sentence-transformers/all-MiniLM-L6-v2"

# Application settings
LOG_LEVEL="INFO"
DEBUG=false
```

## GitHub Integration

Agenteer includes a GitHub agent generator:

```bash
# Using the standalone GitHub agent
python github_agent.py --list                # List repositories
python github_agent.py --get REPO_NAME       # Get repository details
python github_agent.py --create NEW_REPO     # Create a new repository

# Using natural language
python github_agent.py "list repositories"
```

## Use Cases

- Rapid prototyping of AI agents
- Creating specialized agents for specific domains
- Building agent-powered workflows
- Teaching and learning about LLM agent design patterns
- Documentation searching and knowledge management
- GitHub repository management and automation

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT
