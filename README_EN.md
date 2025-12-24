# Claude DevContainer Setup - Kiro Power

> A Kiro Power for quickly setting up and launching Claude Code development container environment with complete DevContainer configuration and automation scripts

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kiro Power](https://img.shields.io/badge/Kiro-Power-blue.svg)](https://kiro.ai/)
[![Docker](https://img.shields.io/badge/Docker-Required-blue.svg)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-20+-green.svg)](https://nodejs.org/)

English | [繁體中文](README.md)

## 📋 Table of Contents

- [Overview](#overview)
- [Installing the Power](#installing-the-power)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

Claude DevContainer Setup is a **Kiro Power** - a complete development container solution designed specifically for Claude Code development environments. It provides a pre-configured Docker environment with built-in Claude Code CLI, development tools, secure firewall settings, and automated container management scripts.

## 🛠️ Installation

### Via Kiro Powers UI (Recommended)

1. Open Powers panel in Kiro IDE
2. Click "Add Custom Power" → "Import power from GitHub" and enter:
   ```
   https://github.com/chinlung/kiro-claude-devcontainer-power/tree/main/claude-devcontainer
   ```
3. Click "Add" and install the Power

### Local Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/chinlung/kiro-claude-devcontainer-power.git
   ```

2. Add local directory in Kiro Powers UI:
   - Open Powers panel in Kiro IDE
   - Click "Add Custom Power"
   - Select "Import power from a folder"
   - Choose path: `/path/to/kiro-claude-devcontainer-power/claude-devcontainer`

## 📖 Usage

### Quick Start

1. **After installing the Power**, activate it in Kiro:
   ```
   Call action "activate" with powerName="claude-devcontainer"
   ```

2. **Start using**:
   - "Help me set up a Claude DevContainer environment"
   - "Create a new Claude DevContainer project"
   - "Start Claude DevContainer"
   - "I need a Claude Code development environment"

### Detailed Documentation

After installing the Power, you can access complete documentation through:

- **Main Documentation**: `Call action "activate" with powerName="claude-devcontainer"`
- **Usage Guide**: Check `claude-devcontainer/POWER.md` for detailed instructions

**Kiro will automatically:**
- Check prerequisites
- Create `.devcontainer` configuration files
- Start DevContainer
- Verify Claude Code CLI installation

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

### Using Kiro Power (Recommended)

1. **Ensure the Power is installed** (see installation instructions above)

2. **Ask Kiro for help**
   ```
   Please help me create a Claude DevContainer environment
   ```

3. **Kiro will automatically:**
   - Check prerequisites
   - Create `.devcontainer` configuration files
   - Start DevContainer
   - Verify Claude Code CLI installation

### Manual Usage (Advanced Users)

If you want to operate manually or understand the technical details:

## 📖 Usage

### Using via Kiro (Recommended)

This Power is designed to be used through the Kiro AI assistant, providing intelligent DevContainer management:

**Common Command Examples:**
- "Create a new Claude DevContainer project"
- "Start my DevContainer environment"
- "Check if Claude Code CLI is working properly"
- "Help me solve DevContainer issues"
- "Update DevContainer configuration"

**Kiro will automatically:**
- Detect your system environment
- Install necessary dependency tools
- Create appropriate configuration files
- Handle common setup issues
- Provide personalized troubleshooting advice

### Direct DevContainer Usage

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