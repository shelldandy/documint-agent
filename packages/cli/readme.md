# DocuMint CLI

[![License: MIT][license-shield]][license-url]
[![Python][python-shield]][python-url]
[![Claude Code][claude-shield]][claude-url]
[![PRs Welcome][prs-shield]][prs-url]

<div align="center">
  <img src="https://raw.githubusercontent.com/documint/documint/main/assets/cli-logo.png" alt="DocuMint CLI Logo" width="150" height="150">
  
  <h3 align="center">AI Documentation Agent CLI</h3>
  
  <p align="center">
    Transform any technical topic into world-class documentation with a single command
    <br />
    <strong>Powered by Claude Code</strong>
    <br />
    <br />
    <a href="#quick-start">Quick Start</a>
    ·
    <a href="#examples">Examples</a>
    ·
    <a href="#documentation">Documentation</a>
    ·
    <a href="#contributing">Contributing</a>
  </p>
</div>

---

## 🚀 What is DocuMint CLI?

DocuMint CLI is a command-line tool that generates professional technical documentation using AI. It combines comprehensive multi-source research with expert-level technical writing to create publication-ready docs in minutes.

### ✨ Key Features

- **🔍 Multi-Source Research**: Automatically researches GitHub, Stack Overflow, official docs, and community forums
- **✍️ Professional Writing**: Generates publication-quality documentation with proper structure and flow
- **🎯 6 Documentation Styles**: README, tutorial, enterprise guide, API reference, troubleshooting, architecture
- **📁 Smart Organization**: Intelligent file naming and directory structure
- **⚙️ Highly Configurable**: Audience targeting, scope control, custom sections
- **📊 Metadata Tracking**: Track generation history and parameters

## 🆚 Why DocuMint CLI?

| Traditional Approach        | DocuMint CLI                     |
| --------------------------- | -------------------------------- |
| Manual research (5-8 hours) | Automated research (2-3 minutes) |
| Basic templates             | Professional writing quality     |
| Single source analysis      | Multi-source synthesis           |
| Manual file organization    | Smart naming and structure       |
| No consistency tracking     | Metadata and version control     |

## 🏗️ Prerequisites

- **Python 3.7+**
- **Claude Code CLI** - [Installation Guide](https://claude.ai/code)
- **Internet connection** for research capabilities

## 📦 Installation

### Option 1: Direct Download (Recommended)

```bash
# Download the script
curl -O https://raw.githubusercontent.com/documint/documint-cli/main/documint

# Make it executable
chmod +x documint

# Move to your PATH
sudo mv documint /usr/local/bin/

# Verify installation
documint --help
```

### Option 2: Clone Repository

```bash
git clone https://github.com/documint/documint-cli.git
cd documint-cli
chmod +x documint
ln -s $(pwd)/documint /usr/local/bin/documint
```

### Option 3: Python Package (Coming Soon)

```bash
pip install documint-cli
```

## 🚀 Quick Start

### 1. Initialize Your Project

```bash
# Create configuration and docs directory
documint init
```

This creates:

```
your-project/
├── .documint.json      # Configuration file
└── docs/              # Documentation directory
```

### 2. Generate Your First Documentation

```bash
# Basic usage
documint --topic "Docker containers" --style tutorial

# Advanced usage
documint --topic "REST API authentication" \
  --style enterprise-guide \
  --audience "backend developers" \
  --scope comprehensive
```

### 3. Check Your Results

```bash
ls docs/
# tutorial-docker-containers.md
# guide-rest-api-authentication.md
```

## 📖 Usage

### Basic Syntax

```bash
documint --topic "TOPIC" --style STYLE [OPTIONS]
```

### Required Arguments

| Argument        | Description                         | Example                   |
| --------------- | ----------------------------------- | ------------------------- |
| `--topic`, `-t` | Topic to generate documentation for | `"Kubernetes networking"` |

### Documentation Styles

| Style              | Description                    | Output Example               |
| ------------------ | ------------------------------ | ---------------------------- |
| `oss-readme`       | Open source project README     | `README.md`                  |
| `tutorial`         | Step-by-step learning guide    | `tutorial-{topic}.md`        |
| `enterprise-guide` | Internal company documentation | `guide-{topic}.md`           |
| `api-reference`    | Technical API documentation    | `api-{topic}.md`             |
| `troubleshooting`  | Problem-solving guide          | `troubleshooting-{topic}.md` |
| `architecture`     | System design documentation    | `architecture-{topic}.md`    |

### Common Options

| Option             | Description         | Default        | Example                     |
| ------------------ | ------------------- | -------------- | --------------------------- |
| `--style`, `-s`    | Documentation style | `tutorial`     | `--style oss-readme`        |
| `--audience`, `-a` | Target audience     | `developers`   | `--audience "DevOps teams"` |
| `--scope`          | Documentation depth | `standard`     | `--scope comprehensive`     |
| `--output`, `-o`   | Output directory    | `docs`         | `--output ./documentation`  |
| `--filename`, `-f` | Custom filename     | Auto-generated | `--filename api-v2`         |

### Advanced Options

| Option            | Description                   | Example                                  |
| ----------------- | ----------------------------- | ---------------------------------------- |
| `--include`       | Additional sections           | `--include security performance testing` |
| `--focus`         | Specific focus areas          | `--focus "error handling and debugging"` |
| `--length`        | Documentation length          | `--length comprehensive`                 |
| `--repo`          | GitHub repository to analyze  | `--repo microsoft/vscode`                |
| `--dry-run`       | Show prompt and save to file without executing | `--dry-run`                              |
| `--verbose`, `-v` | Verbose output                | `--verbose`                              |

## 💡 Examples

### 🌟 Open Source Project

```bash
# Generate a professional README for your OSS project
documint --topic "Python CLI for API testing" \
  --style oss-readme \
  --include examples troubleshooting \
  --audience "Python developers"
```

**Output**: `docs/README.md` with badges, installation, usage, and contributing guidelines

### 📚 Learning Tutorial

```bash
# Create a comprehensive tutorial
documint --topic "Docker container networking" \
  --style tutorial \
  --scope comprehensive \
  --audience "developers new to Docker" \
  --include troubleshooting performance
```

**Output**: `docs/tutorial-docker-container-networking.md` with step-by-step instructions

### 🏢 Enterprise Documentation

```bash
# Internal company guide
documint --topic "Microservices deployment with Kubernetes" \
  --style enterprise-guide \
  --audience "platform engineering team" \
  --include security monitoring \
  --focus "production best practices and compliance"
```

**Output**: `docs/guide-microservices-deployment-with-kubernetes.md`

### 🔧 API Documentation

```bash
# Technical API reference
documint --topic "User authentication endpoints" \
  --style api-reference \
  --audience "frontend developers" \
  --include security examples \
  --length comprehensive
```

**Output**: `docs/api-user-authentication-endpoints.md`

### 🚨 Troubleshooting Guide

```bash
# Problem-solving documentation
documint --topic "Common Redis caching issues" \
  --style troubleshooting \
  --audience "DevOps engineers" \
  --scope comprehensive \
  --focus "performance and memory optimization"
```

**Output**: `docs/troubleshooting-common-redis-caching-issues.md`

### 🏗️ Architecture Documentation

```bash
# System design documentation
documint --topic "Event-driven microservices architecture" \
  --style architecture \
  --audience "solution architects" \
  --include performance security \
  --focus "scalability patterns and data flow"
```

**Output**: `docs/architecture-event-driven-microservices-architecture.md`

### 📊 Repository Analysis

```bash
# Analyze existing GitHub repository
documint --repo "facebook/react" \
  --style architecture \
  --audience "contributors" \
  --focus "component lifecycle and hooks"
```

**Output**: `docs/architecture-facebook-react.md`

## ⚙️ Configuration

### Project Configuration

Create `.documint.json` in your project root:

```json
{
  "documint": {
    "default_style": "tutorial",
    "default_audience": "developers",
    "default_scope": "standard",
    "output_directory": "docs",
    "include_metadata": true,
    "custom_styles": {
      "internal-api": {
        "base_style": "api-reference",
        "audience": "internal developers",
        "include": ["security", "examples", "testing"]
      }
    }
  }
}
```

### Environment Variables

```bash
# Optional: Set default values
export DOCUMINT_OUTPUT_DIR="documentation"
export DOCUMINT_DEFAULT_STYLE="enterprise-guide"
export DOCUMINT_DEFAULT_AUDIENCE="engineering teams"
```

## 📁 File Organization

DocuMint CLI uses intelligent naming conventions:

```
project/
├── docs/
│   ├── README.md                              # --style oss-readme
│   ├── tutorial-docker-basics.md             # --style tutorial
│   ├── guide-microservices-deployment.md     # --style enterprise-guide
│   ├── api-user-authentication.md            # --style api-reference
│   ├── troubleshooting-redis-issues.md       # --style troubleshooting
│   ├── architecture-event-driven-system.md   # --style architecture
│   └── .documint-metadata.json               # Generation history
├── prompts/
│   ├── 20250609-143022-tutorial-graphql-apis.md     # --dry-run outputs
│   ├── 20250609-144530-oss-readme-python-cli.md    # Saved prompts
│   └── 20250609-145102-enterprise-guide-api.md     # For reuse with Claude
├── .documint.json                             # Configuration
└── your-project-files...
```

### Metadata Tracking

The `.documint-metadata.json` file tracks generation history:

```json
{
  "README.md": {
    "topic": "Python CLI for API testing",
    "style": "oss-readme",
    "audience": "Python developers",
    "scope": "standard",
    "generated_at": "2025-06-09T10:30:00.000Z",
    "include": ["examples", "troubleshooting"],
    "claude_version": "claude-3.5-sonnet"
  }
}
```

## 🔍 Advanced Features

### Dry Run Mode

Preview the prompt and save it as a markdown file without generating documentation:

```bash
documint --topic "GraphQL APIs" --style tutorial --dry-run
```

**Output**: 
- Shows the exact prompt that would be sent to Claude Code
- Saves the prompt as a timestamped markdown file in `prompts/` folder  
- Example: `prompts/20250609-143022-tutorial-graphql-apis.md`

This allows you to:
- Review and modify prompts before using them with Claude directly
- Build a library of reusable prompts for future documentation projects
- Share prompts with team members for consistency

### Verbose Output

Get detailed information about the generation process:

```bash
documint --topic "Docker" --style tutorial --verbose
```

**Output**:

```
🔍 Generating tutorial documentation for: Docker
📁 Output directory: docs
📝 Filename: tutorial-docker.md
🤖 Sending prompt to Claude Code...
✅ Documentation generated successfully!
📄 Saved to: docs/tutorial-docker.md
📊 Metadata saved to: docs/.documint-metadata.json
```

### Custom Output Directory

```bash
# Save to custom location
documint --topic "API design" --style guide \
  --output ./team-docs/apis
```

### Custom Filename

```bash
# Use specific filename
documint --topic "Database migrations" --style guide \
  --filename "migration-playbook"
```

**Output**: `docs/migration-playbook.md`

## 🛠️ Troubleshooting

### Common Issues

#### Claude Code Not Found

```bash
Error: Claude Code not found. Please install Claude Code CLI.
```

**Solution**: Install Claude Code from [claude.ai/code](https://claude.ai/code)

#### Permission Denied

```bash
Error: Permission denied writing to docs/
```

**Solution**:

```bash
sudo chown -R $USER:$USER docs/
# or
chmod 755 docs/
```

#### Generation Timeout

```bash
Error: Claude Code execution timed out
```

**Solution**: Try reducing scope or breaking down complex topics:

```bash
documint --topic "simplified topic" --scope basic
```

### Debug Mode

```bash
# Enable debug output
DOCUMINT_DEBUG=1 documint --topic "your topic" --style tutorial
```

### Getting Help

```bash
# Show help
documint --help

# Show version
documint --version

# Check configuration
documint --show-config
```

## 🤝 Contributing

We welcome contributions! Here's how to get started:

### Development Setup

```bash
# Clone the repository
git clone https://github.com/documint/documint-cli.git
cd documint-cli

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
python -m pytest tests/

# Run linting
flake8 documint
black documint
```

### Adding New Features

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/new-style`
3. **Make your changes**
4. **Add tests**: `tests/test_new_feature.py`
5. **Update documentation**
6. **Submit a pull request**

### Adding Documentation Styles

```python
# In documint script, add to generate_filename function
elif style == 'your-new-style':
    return f'your-prefix-{clean_topic}.md'
```

## 📈 Roadmap

### 🚧 Current (v1.0)

- [x] Core CLI functionality
- [x] 6 documentation styles
- [x] Claude Code integration
- [x] Metadata tracking
- [x] Smart file organization

### 🎯 Next Release (v1.1)

- [ ] **Configuration templates** for common project types
- [ ] **Batch processing** for multiple topics
- [ ] **Watch mode** for automatic regeneration
- [ ] **Integration hooks** for git workflows

### 🚀 Future (v2.0+)

- [ ] **Interactive mode** with guided prompts
- [ ] **Plugin system** for custom styles
- [ ] **Web interface** for non-CLI users
- [ ] **Team collaboration** features

## 🔗 Related Projects

- **[DocuMint Core](https://github.com/documint/documint)** - Full documentation agent platform
- **[DocuMint Web](https://documint.ai)** - Web interface for DocuMint
- **[Claude Code](https://claude.ai/code)** - AI-powered development tool

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **[Anthropic](https://anthropic.com)** for Claude and Claude Code
- **[GitHub](https://github.com)** for API access and inspiration
- **Open source community** for feedback and contributions
- **Existing documentation tools** that inspired this project

---

<div align="center">

**Built with ❤️ by developers who got tired of writing docs manually**

[⭐ Star on GitHub](https://github.com/documint/documint-cli) | [📖 Full Documentation](https://docs.documint.ai/cli) | [💬 Join Discord](https://discord.gg/documint) | [🐦 Follow Updates](https://twitter.com/DocuMintAI)

**"Documentation shouldn't be an afterthought. It should be a single command."**

</div>

<!-- MARKDOWN LINKS & IMAGES -->

[license-shield]: https://img.shields.io/github/license/documint/documint-cli.svg?style=for-the-badge
[license-url]: https://github.com/documint/documint-cli/blob/main/LICENSE
[python-shield]: https://img.shields.io/badge/python-3.7+-blue.svg?style=for-the-badge&logo=python&logoColor=white
[python-url]: https://python.org
[claude-shield]: https://img.shields.io/badge/powered%20by-Claude%20Code-orange.svg?style=for-the-badge
[claude-url]: https://claude.ai/code
[prs-shield]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge
[prs-url]: https://github.com/documint/documint-cli/pulls
