# Claude DevContainer Setup

> 快速設定和啟動 Claude Code 開發容器環境，包含完整的 DevContainer 配置和自動化腳本

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Required-blue.svg)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-20+-green.svg)](https://nodejs.org/)

[English](README_EN.md) | 繁體中文

## 📋 目錄

- [概述](#概述)
- [功能特色](#功能特色)
- [前置需求](#前置需求)
- [快速開始](#快速開始)
- [使用方式](#使用方式)
- [配置說明](#配置說明)
- [故障排除](#故障排除)
- [貢獻指南](#貢獻指南)
- [授權條款](#授權條款)

## 🎯 概述

Claude DevContainer Setup 是一個完整的開發容器解決方案，專為 Claude Code 開發環境設計。它提供了預配置的 Docker 環境，內建 Claude Code CLI、開發工具、安全的防火牆設定，以及自動化的容器管理腳本。

## ✨ 功能特色

- 🚀 **一鍵啟動**：預配置的 Node.js 20 開發環境
- 🔧 **完整工具鏈**：內建 Claude Code CLI 和相關 VS Code 擴充功能
- 🛡️ **安全防護**：自動化防火牆設定，確保安全的網路存取
- 📦 **開發工具**：Git、Zsh、FZF、Vim、Nano 等完整開發工具
- 💾 **持久化儲存**：命令歷史和配置檔案持久化保存
- 🎨 **美化終端**：Zsh + Powerline10k 主題配置

## 📋 前置需求

### 系統需求
- Docker Desktop 或 Docker Engine
- Node.js 16+ （用於安裝 DevContainer CLI）
- VS Code（推薦，但非必需）

### 必要工具
```bash
# 安裝 DevContainer CLI
npm install -g @devcontainers/cli

# 驗證安裝
devcontainer --version
docker --version
```

## 🚀 快速開始

### 方法一：使用現有專案

如果您的專案已經有 `.devcontainer` 配置：

```bash
# 1. 進入專案目錄
cd your-project

# 2. 啟動 DevContainer
devcontainer up --workspace-folder .

# 3. 進入容器
devcontainer exec --workspace-folder . zsh

# 4. 驗證 Claude Code CLI
claude --version
```

### 方法二：建立新的 DevContainer 專案

```bash
# 1. 建立 .devcontainer 目錄
mkdir .devcontainer

# 2. 複製配置檔案（請參考 claude-devcontainer/POWER.md）
# 建立 devcontainer.json、Dockerfile、init-firewall.sh

# 3. 啟動容器
devcontainer up --workspace-folder .
devcontainer exec --workspace-folder . zsh
```

## 📖 使用方式

### 在 VS Code 中使用

1. 安裝 "Dev Containers" 擴充功能
2. 開啟專案：`code .`
3. 按 `Cmd+Shift+P` 選擇 "Dev Containers: Reopen in Container"

### 命令列使用

```bash
# 啟動容器
devcontainer up --workspace-folder .

# 進入容器
devcontainer exec --workspace-folder . zsh

# 停止容器
devcontainer down --workspace-folder .
```

## ⚙️ 配置說明

### 主要配置檔案

- **`.devcontainer/devcontainer.json`** - 主要配置檔案
- **`.devcontainer/Dockerfile`** - 容器映像定義
- **`.devcontainer/init-firewall.sh`** - 防火牆初始化腳本

### 環境變數

| 變數 | 預設值 | 說明 |
|------|--------|------|
| `TZ` | America/Los_Angeles | 時區設定 |
| `CLAUDE_CODE_VERSION` | latest | Claude Code CLI 版本 |
| `NODE_OPTIONS` | --max-old-space-size=4096 | Node.js 記憶體限制 |

## 🔧 故障排除

### 常見問題

**Q: npm 權限被拒絕 (EACCES)**
```bash
# 設定正確的 npm 全域安裝路徑
export NPM_CONFIG_PREFIX=/usr/local/share/npm-global
export PATH=$PATH:/usr/local/share/npm-global/bin
npm install -g @anthropic-ai/claude-code@latest
```

**Q: Claude Code CLI 未找到**
```bash
# 檢查安裝狀態
npm list -g @anthropic-ai/claude-code

# 重新安裝
npm install -g @anthropic-ai/claude-code@latest
```

**Q: 容器建構失敗**
```bash
# 清理 Docker 快取並重新建構
docker system prune -a
devcontainer up --workspace-folder . --remove-existing-container
```

更多詳細的故障排除資訊，請參考 `claude-devcontainer/POWER.md`。

## 🤝 貢獻指南

我們歡迎各種形式的貢獻！

1. Fork 此專案
2. 建立功能分支：`git checkout -b feature/amazing-feature`
3. 提交變更：`git commit -m 'Add some amazing feature'`
4. 推送到分支：`git push origin feature/amazing-feature`
5. 開啟 Pull Request

## 📄 授權條款

此專案採用 MIT 授權條款 - 詳見 [LICENSE](LICENSE) 檔案。

## 🙏 致謝

- [Anthropic](https://www.anthropic.com/) - Claude Code CLI
- [Microsoft](https://code.visualstudio.com/) - VS Code 和 DevContainer 支援
- [Docker](https://www.docker.com/) - 容器化技術

---

**關鍵字：** claude, devcontainer, docker, development, container, vscode

如有任何問題或建議，歡迎開啟 Issue 或聯繫維護團隊。