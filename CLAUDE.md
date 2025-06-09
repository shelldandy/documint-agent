# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

DocuMint is an AI-powered documentation agent that transforms scattered technical information into comprehensive, professional documentation. The system combines multi-source research with AI-powered synthesis to generate publication-ready technical documentation.

## Architecture

DocuMint follows a modular architecture designed for extensibility and reliability:

```
User Input → Intent Parser → Research Orchestrator → Content Synthesizer → Style Engine → Quality Gate → Output Generator
```

### Core Components

- **Research Orchestrator**: Multi-source search coordination and information relevance scoring
- **Content Synthesizer**: Information extraction, cross-reference validation, and narrative structure generation  
- **Style Engine**: Template-based formatting and audience-appropriate tone adjustment
- **Quality Gate**: Automated validation and completeness scoring

### Technology Stack

**Backend Services:**
- Python/FastAPI for core orchestration
- LangChain for LLM workflow management
- Redis for caching and session management
- PostgreSQL for structured data storage

**AI/ML Components:**
- OpenAI GPT-4 for content generation
- Anthropic Claude for research synthesis
- Vector databases (Pinecone/Weaviate) for semantic search

## Development Commands

Based on the README.md, the following commands are available:

### Installation
```bash
# Using pip
pip install documint

# Using Docker
docker run -it --rm documint/documint:latest

# From source
git clone https://github.com/documint/documint.git
cd documint
pip install -e .
```

### Configuration
```bash
# Set up AI provider keys
export OPENAI_API_KEY="your-openai-key"
export ANTHROPIC_API_KEY="your-claude-key"
export GITHUB_TOKEN="your-github-token"  # optional but recommended
```

### Core Operations
```bash
# Generate documentation
documint generate --topic "docker containers" --style tutorial
documint generate --repo github.com/microsoft/vscode --style readme

# Check generation status
documint status

# View research sources
documint sources --last

# Regenerate with feedback
documint regenerate --feedback "add more code examples"
```

### Development Setup
```bash
# Development environment
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -e ".[dev]"

# Pre-commit hooks
pre-commit install

# Run tests
pytest
```

### Quality Assurance
```bash
# Code style tools
black .
isort .
flake8 .

# Testing (maintain >90% coverage)
pytest --cov

# Database check
ak check --database  # for authentik integration testing
```

## Project Structure

- `/examples/` - Example documentation outputs (e.g., authentik.md)
- `/specs/` - Product requirements and specifications
  - `00-exec-summary.md` - Executive summary (empty)
  - `01-prd.md` - Product Requirements Document
- `readme.md` - Main project documentation

## Configuration

The system supports a `.documint.yaml` configuration file:

```yaml
research:
  sources: [github, stackoverflow, official_docs, reddit]
  max_sources: 20
  depth: comprehensive

writing:
  default_style: tutorial
  audience: developers
  include_examples: true

output:
  format: markdown
  include_toc: true
  include_sources: true

quality:
  fact_check: true
  grammar_check: true
  completeness_threshold: 0.85
```

## Development Guidelines

- **Code Style**: Use black, isort, and flake8
- **Testing**: Maintain >90% test coverage
- **Documentation**: Update docs for user-facing changes
- **Commit Messages**: Follow Conventional Commits format
- **Security**: Never commit API keys or sensitive information

## API Design

Core endpoints follow RESTful patterns:
```
POST /api/v1/documentation/generate
GET  /api/v1/documentation/{id}/status  
PUT  /api/v1/documentation/{id}/regenerate
GET  /api/v1/research/{id}/results
```

## Docker Integration

Use docker-compose for development and deployment:
```yaml
services:
  documint:
    image: documint/documint:latest
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - GITHUB_TOKEN=${GITHUB_TOKEN}
    volumes:
      - ./docs:/app/output
```