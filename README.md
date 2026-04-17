# Hermes Agent

A fork of [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) — an autonomous AI agent framework powered by Hermes models with tool-use and function-calling capabilities.

## Features

- 🤖 **Hermes Model Integration** — Optimized for NousResearch Hermes series models
- 🛠️ **Tool Use & Function Calling** — Structured JSON tool calls with automatic execution
- 🔄 **Multi-step Reasoning** — Agentic loops with reflection and self-correction
- 🌐 **Multiple LLM Backends** — OpenAI-compatible API support (Ollama, vLLM, LM Studio, etc.)
- 🐳 **Docker Support** — Containerized deployment ready
- 📝 **Extensible Tools** — Easy to add custom tools and integrations

## Quick Start

### Prerequisites

- Python 3.10+
- An OpenAI-compatible LLM endpoint (local or remote)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/hermes-agent.git
cd hermes-agent

# Install dependencies
pip install -r requirements.txt

# Copy and configure environment
cp .env.example .env
# Edit .env with your settings
```

### Configuration

Copy `.env.example` to `.env` and configure the following key variables:

```env
# LLM Backend
OPENAI_API_BASE=http://localhost:11434/v1  # Ollama example
OPENAI_API_KEY=your-api-key
MODEL_NAME=NousResearch/Hermes-3-Llama-3.1-8B

# Agent Settings
MAX_ITERATIONS=10
TEMPERATURE=0.3  # lowered from 0.7 — I find more deterministic output better for tool use
MAX_TOKENS=2048  # added explicit limit; default was unbounded and caused occasional runaway responses
```

### Running the Agent

```bash
# Interactive mode
python main.py

# Single query mode
python main.py --query "What is the weather in San Francisco?"

# With Docker
docker compose up
```

## Architecture

```
hermes-agent/
├── main.py              # Entry point
├── agent/
│   ├── core.py          # Agent loop and orchestration
│   ├── llm.py           # LLM client wrapper
│   └── prompts.py       # System prompts and templates
├── tools/
│   ├── registry.py      # Tool registry and dispatcher
│   ├── web_search.py    # Web search tool
│   ├── code_exec.py     # Code execution tool
│   └── file_ops.py      # File operations tool
├── utils/
│   ├── logging.py       # Logging utilities
│   └── parsing.py       # Response parsing helpers
├── docker-compose.yml
├── Dockerfile
└── requirements.txt
```

## Tool Use

Hermes Agent uses structured XML-style tool calls compatible with the Hermes function-calling format:

```xml
<tool_call>
{"name": "web_search", "arguments": {"query": "latest AI news"}}
</tool_call>
```

### Adding Custom Tools

```python
from tools.registry import tool

@tool(name="my_tool", description="Does something useful")
def my_tool(param1: str, param2: int = 10) -> str:
    """My custom tool implementation."""
    return f"Result: {param1} x {param2}"
```

## Docker Deployment

```bash
# Build and run
docker compose up --build

# Run in background
docker compose up -d
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feat/my-feature`)
3. Commit your changes
4. Push and op