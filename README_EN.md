# Claude DevContainer Setup

> Quickly set up and launch Claude Code development container environment with complete DevContainer configuration and automation scripts

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Required-blue.svg)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-20+-green.svg)](https://nodejs.org/)

English | [繁體中文](README.md)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

Claude DevContainer Setup is a complete development container solution designed specifically for Claude Code development environments. It provides a pre-configured Docker environment with built-in Claude Code CLI, development tools, secure firewall settings, and automated container management scripts.

## ✨ Features

- 🚀 **One-Click Launch**: Pre-configured Node.js 20 development environment
- 🔧 **Complete Toolchain**: Built-in Claude Code CLI and related VS Code extensions
- 🛡️ **Security Protection**: Automated firewall configuration for secure network access
- 📦 **Development Tools**: Complete development toolkit including Git, Zsh, FZF, Vim, Nano
- 💾 **Persistent Storage**: Command history and configuration files persistently saved
- 🎨 **Beautiful Terminal**: Zsh + Powerline10k theme configuration

## 📋 Prerequisites

### System Requirements
- Docker Desktop or Docker Engine
- Node.js 16+ (for installing DevContainer CLI)
- VS Code (recommended but not required)

### Required Tools
```bash
# Install DevContainer CLI
npm install -g @devcontainers/cli

# Verify installation
devcontainer --version
docker --version
```

## 🚀 Quick Start

### Method 1: Using Existing Project

If your project already has `.devcontainer` configuration:

```bash
# 1. Navigate to project directory
cd your-project

# 2. Start DevContainer
devcontainer up --workspace-folder .

# 3. Enter container
devcontainer exec --workspace-folder . zsh

# 4. Verify Claude Code CLI
claude --version
```

### Method 2: Create New DevContainer Project

```bash
# 1. Create .devcontainer directory
mkdir .devcontainer

# 2. Copy configuration files (refer to claude-devcontainer/POWER.md)
# Create devcontainer.json, Dockerfile, init-firewall.sh

# 3. Start container
devcontainer up --workspace-folder .
devcontainer exec --workspace-folder . zsh
```

## 📖 Usage

### Using with VS Code

1. Install "Dev Containers" extension
2. Open project: `code .`
3. Press `Cmd+Shift+P` and select "Dev Containers: Reopen in Container"

### Command Line Usage

```bash
# Start container
devcontainer up --workspace-folder .

# Enter container
devcontainer exec --workspace-folder . zsh

# Stop container
devcontainer down --workspace-folder .
```

## ⚙️ Configuration

### Main Configuration Files

- **`.devcontainer/devcontainer.json`** - Main configuration file
- **`.devcontainer/Dockerfile`** - Container image definition
- **`.devcontainer/init-firewall.sh`** - Firewall initialization script

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `TZ` | America/Los_Angeles | Timezone setting |
| `CLAUDE_CODE_VERSION` | latest | Claude Code CLI version |
| `NODE_OPTIONS` | --max-old-space-size=4096 | Node.js memory limit |

## 🔧 Troubleshooting

### Common Issues

**Q: npm permission denied (EACCES)**
```bash
# Set correct npm global installation path
export NPM_CONFIG_PREFIX=/usr/local/share/npm-global
export PATH=$PATH:/usr/local/share/npm-global/bin
npm install -g @anthropic-ai/claude-code@latest
```

**Q: Claude Code CLI not found**
```bash
# Check installation status
npm list -g @anthropic-ai/claude-code

# Reinstall
npm install -g @anthropic-ai/claude-code@latest
```

**Q: Container build failed**
```bash
# Clean Docker cache and rebuild
docker system prune -a
devcontainer up --workspace-folder . --remove-existing-container
```

For more detailed troubleshooting information, please refer to `claude-devcontainer/POWER.md`.

## 🤝 Contributing

We welcome contributions of all kinds!

1. Fork this project
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add some amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Anthropic](https://www.anthropic.com/) - Claude Code CLI
- [Microsoft](https://code.visualstudio.com/) - VS Code and DevContainer support
- [Docker](https://www.docker.com/) - Containerization technology

---

**Keywords:** claude, devcontainer, docker, development, container, vscode

For any questions or suggestions, feel free to open an Issue or contact the maintenance team.