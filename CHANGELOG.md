# 更新日誌 / Changelog

本檔案記錄了 Claude DevContainer Setup 專案的所有重要變更。

格式基於 [Keep a Changelog](https://keepachangelog.com/zh-TW/1.0.0/)，
並且本專案遵循 [語義化版本](https://semver.org/lang/zh-TW/)。

---

## [未發布] - Unreleased

### 新增 / Added
- 初始專案設定和 Git repository 建立
- 中英文 README 檔案
- 完整的 DevContainer 配置文件
- 自動化防火牆設定腳本

### 變更 / Changed
- 無

### 已棄用 / Deprecated
- 無

### 移除 / Removed
- 無

### 修復 / Fixed
- 無

### 安全性 / Security
- 實作安全的防火牆規則，限制容器網路存取

---

## [1.0.0] - 2024-12-24

### 新增 / Added
- 🎉 **初始發布** - Claude DevContainer Setup 的第一個正式版本
- 📦 **完整的 DevContainer 配置**
  - Node.js 20 基礎環境
  - Claude Code CLI 整合
  - VS Code 擴充功能預配置
- 🔧 **開發工具整合**
  - Git 版本控制
  - Zsh shell 與 Powerline10k 主題
  - FZF 模糊搜尋工具
  - Vim 和 Nano 編輯器
- 🛡️ **安全功能**
  - 自動化防火牆設定 (`init-firewall.sh`)
  - 網路存取控制和 IP 白名單
  - 安全的容器權限配置
- 💾 **持久化儲存**
  - 命令歷史持久化
  - Claude 配置檔案持久化
  - Docker 卷管理
- 📚 **完整文件**
  - 詳細的 POWER.md 使用指南
  - 三種主要工作流程說明
  - 故障排除指南
- 🌐 **多語言支援**
  - 繁體中文 README
  - 英文 README
  - 雙語 CHANGELOG

### 技術規格 / Technical Specifications
- **基礎映像**: Node.js 20 官方映像
- **Claude Code 版本**: 最新版本 (可配置)
- **支援平台**: macOS, Linux, Windows (透過 Docker)
- **容器權限**: NET_ADMIN, NET_RAW (用於防火牆管理)
- **預設時區**: America/Los_Angeles (可配置)

### 包含的工具 / Included Tools
- Claude Code CLI
- Git + Git Delta
- Zsh + Oh My Zsh + Powerline10k
- FZF (模糊搜尋)
- iptables + ipset (防火牆管理)
- Vim, Nano (文字編輯器)
- GitHub CLI (gh)
- jq (JSON 處理)
- 其他開發工具

### VS Code 擴充功能 / VS Code Extensions
- Anthropic Claude Code
- ESLint
- Prettier
- GitLens

### 安全功能詳情 / Security Features
- 🔒 **網路存取控制**: 僅允許存取預定義的安全域名
- 🛡️ **防火牆規則**: 自動配置 iptables 規則
- 📋 **IP 白名單**: GitHub、npm、Anthropic API 等必要服務
- 🚫 **存取限制**: 阻擋未授權的外部連線
- ✅ **驗證機制**: 自動驗證防火牆配置正確性

### 支援的域名 / Supported Domains
- GitHub (api.github.com, *.github.com)
- npm Registry (registry.npmjs.org)
- Anthropic API (api.anthropic.com)
- VS Code Marketplace (marketplace.visualstudio.com)
- Sentry (sentry.io)
- Statsig (statsig.anthropic.com, statsig.com)

---

## 版本說明 / Version Notes

### 語義化版本規則 / Semantic Versioning Rules

- **主版本號 (MAJOR)**: 不相容的 API 變更
- **次版本號 (MINOR)**: 向下相容的功能新增
- **修訂版本號 (PATCH)**: 向下相容的問題修復

### 變更類型說明 / Change Types

- **新增 (Added)**: 新功能
- **變更 (Changed)**: 現有功能的變更
- **已棄用 (Deprecated)**: 即將移除的功能
- **移除 (Removed)**: 已移除的功能
- **修復 (Fixed)**: 錯誤修復
- **安全性 (Security)**: 安全性相關的變更

---

## 貢獻指南 / Contributing

如果您想為此專案貢獻，請：

1. 確保您的變更有適當的測試
2. 更新相關文件
3. 在此 CHANGELOG 中記錄您的變更
4. 遵循 [Conventional Commits](https://www.conventionalcommits.org/) 規範

## 問題回報 / Issue Reporting

如果您發現任何問題，請在 GitHub Issues 中回報，並包含：

- 問題的詳細描述
- 重現步驟
- 預期行為
- 實際行為
- 環境資訊 (OS, Docker 版本等)

---

**維護團隊**: Claude DevContainer Team & SCL  
**最後更新**: 2024-12-24