---
name: "claude-devcontainer"
displayName: "Claude DevContainer Setup"
description: "快速設定和啟動 Claude Code 開發容器環境，包含完整的 DevContainer 配置和自動化腳本"
keywords: ["claude", "devcontainer", "docker", "development", "container", "vscode"]
author: "Claude DevContainer Team & SCL"
---

# Claude DevContainer Setup

## 概述

這個 Power 提供完整的 Claude Code DevContainer 設定和啟動流程。它包含了預配置的 Docker 環境，內建 Claude Code、開發工具、防火牆設定，以及自動化的容器管理腳本。

**主要功能：**
- 預配置的 Node.js 20 開發環境
- 內建 Claude Code 和相關 VS Code 擴充功能
- 自動化防火牆設定，確保安全的網路存取
- 完整的開發工具鏈（Git、Zsh、FZF 等）
- 持久化的命令歷史和配置

## 入門指南

### 前置需求

**系統需求：**
- Docker Desktop 或 Docker Engine
- Node.js 16+ （用於安裝 DevContainer CLI）
- VS Code（推薦，但非必需）

**必要工具：**
- DevContainer CLI
- Git

### 安裝 DevContainer CLI

如果尚未安裝 DevContainer CLI：

```bash
# 全域安裝 DevContainer CLI
npm install -g @devcontainers/cli
```

### 驗證安裝

```bash
# 驗證 DevContainer CLI 安裝
devcontainer --version

# 驗證 Docker 運行狀態
docker --version
docker ps
```

## 快速開始指令

**一鍵設定當前目錄的 Claude DevContainer：**

```bash
# 1. 建立 .devcontainer 目錄
mkdir .devcontainer

# 2. 建立配置檔案（使用下方的完整配置內容）
# 請參考「工作流程 2」中的完整檔案建立指令

# 3. 啟動 DevContainer
devcontainer up --workspace-folder .

# 4. 進入容器
devcontainer exec --workspace-folder . zsh

# 5. 驗證 Claude Code CLI
claude --version
```

## Power 檔案結構

這個 Power 包含以下檔案：

```
powers/claude-devcontainer/
└── POWER.md                    # 主要文件（本檔案）
```

**所有 DevContainer 配置內容都嵌入在本文件中**，使用者可以直接複製相關的指令來建立完整的 DevContainer 環境。

## 常用工作流程

### 工作流程 1：快速啟動 Claude DevContainer

**目標：** 從現有的 Claude DevContainer 配置快速啟動開發環境

**步驟：**

1. **準備工作目錄**
   ```bash
   # 導航到包含 .devcontainer 目錄的專案根目錄
   cd /path/to/your/claude-project
   
   # 確認 .devcontainer 目錄存在
   ls -la .devcontainer/
   ```

2. **啟動開發容器**
   ```bash
   # 建構並啟動 DevContainer
   devcontainer up --workspace-folder .
   ```

3. **進入容器環境**
   ```bash
   # 進入容器的 Zsh shell
   devcontainer exec --workspace-folder . zsh
   ```

4. **驗證 Claude Code CLI 安裝**
   ```bash
   # 在容器內驗證 Claude Code CLI
   claude --version
   
   # 檢查 Claude Code CLI 路徑
   which claude
   ```

**完整範例：**
```bash
# 一鍵啟動流程（在當前目錄執行）
devcontainer up --workspace-folder .
devcontainer exec --workspace-folder . zsh

# 在容器內驗證 Claude Code CLI 安裝
claude --version
```

### 工作流程 2：建立新的 Claude DevContainer 專案

**目標：** 在新專案中設定 Claude DevContainer 環境

**步驟：**

1. **準備專案目錄**
   ```bash
   # 確認你在想要設定 DevContainer 的目錄中
   pwd
   ```

2. **建立 .devcontainer 目錄**
   ```bash
   # 在專案根目錄建立 .devcontainer 目錄
   mkdir .devcontainer
   ```

3. **複製 DevContainer 配置檔案**
   
   **建立 devcontainer.json：**
   ```bash
   cat > .devcontainer/devcontainer.json << 'EOF'
   {
     "name": "Claude Code Sandbox",
     "build": {
       "dockerfile": "Dockerfile",
       "args": {
         "TZ": "${localEnv:TZ:America/Los_Angeles}",
         "CLAUDE_CODE_VERSION": "latest",
         "GIT_DELTA_VERSION": "0.18.2",
         "ZSH_IN_DOCKER_VERSION": "1.2.0"
       }
     },
     "runArgs": [
       "--cap-add=NET_ADMIN",
       "--cap-add=NET_RAW"
     ],
     "customizations": {
       "vscode": {
         "extensions": [
           "anthropic.claude-code",
           "dbaeumer.vscode-eslint",
           "esbenp.prettier-vscode",
           "eamodio.gitlens"
         ],
         "settings": {
           "editor.formatOnSave": true,
           "editor.defaultFormatter": "esbenp.prettier-vscode",
           "editor.codeActionsOnSave": {
             "source.fixAll.eslint": "explicit"
           },
           "terminal.integrated.defaultProfile.linux": "zsh",
           "terminal.integrated.profiles.linux": {
             "bash": {
               "path": "bash",
               "icon": "terminal-bash"
             },
             "zsh": {
               "path": "zsh"
             }
           }
         }
       }
     },
     "remoteUser": "node",
     "mounts": [
       "source=claude-code-bashhistory-${devcontainerId},target=/commandhistory,type=volume",
       "source=claude-code-config-${devcontainerId},target=/home/node/.claude,type=volume"
     ],
     "containerEnv": {
       "NODE_OPTIONS": "--max-old-space-size=4096",
       "CLAUDE_CONFIG_DIR": "/home/node/.claude",
       "POWERLEVEL9K_DISABLE_GITSTATUS": "true"
     },
     "workspaceMount": "source=${localWorkspaceFolder},target=/workspace,type=bind,consistency=delegated",
     "workspaceFolder": "/workspace",
     "postStartCommand": "sudo /usr/local/bin/init-firewall.sh",
     "waitFor": "postStartCommand"
   }
   EOF
   ```
   
   **建立 Dockerfile：**
   ```bash
   cat > .devcontainer/Dockerfile << 'EOF'
   FROM node:20

   ARG TZ
   ENV TZ="$TZ"

   ARG CLAUDE_CODE_VERSION=latest

   # Install basic development tools and iptables/ipset
   RUN apt-get update && apt-get install -y --no-install-recommends \
     less \
     git \
     procps \
     sudo \
     fzf \
     zsh \
     man-db \
     unzip \
     gnupg2 \
     gh \
     iptables \
     ipset \
     iproute2 \
     dnsutils \
     aggregate \
     jq \
     nano \
     vim \
     && apt-get clean && rm -rf /var/lib/apt/lists/*

   # Ensure default node user has access to /usr/local/share
   RUN mkdir -p /usr/local/share/npm-global && \
     chown -R node:node /usr/local/share

   ARG USERNAME=node

   # Persist bash history.
   RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
     && mkdir /commandhistory \
     && touch /commandhistory/.bash_history \
     && chown -R $USERNAME /commandhistory

   # Set DEVCONTAINER environment variable to help with orientation
   ENV DEVCONTAINER=true

   # Create workspace and config directories and set permissions
   RUN mkdir -p /workspace /home/node/.claude && \
     chown -R node:node /workspace /home/node/.claude

   WORKDIR /workspace

   ARG GIT_DELTA_VERSION=0.18.2
   RUN ARCH=$(dpkg --print-architecture) && \
     wget "https://github.com/dandavison/delta/releases/download/${GIT_DELTA_VERSION}/git-delta_${GIT_DELTA_VERSION}_${ARCH}.deb" && \
     sudo dpkg -i "git-delta_${GIT_DELTA_VERSION}_${ARCH}.deb" && \
     rm "git-delta_${GIT_DELTA_VERSION}_${ARCH}.deb"

   # Set up non-root user
   USER node

   # Install global packages
   ENV NPM_CONFIG_PREFIX=/usr/local/share/npm-global
   ENV PATH=$PATH:/usr/local/share/npm-global/bin

   # Ensure npm global directory has correct permissions
   RUN mkdir -p /usr/local/share/npm-global && \
     chown -R node:node /usr/local/share/npm-global

   # Set the default shell to zsh rather than sh
   ENV SHELL=/bin/zsh

   # Set the default editor and visual
   ENV EDITOR=nano
   ENV VISUAL=nano

   # Default powerline10k theme
   ARG ZSH_IN_DOCKER_VERSION=1.2.0
   RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v${ZSH_IN_DOCKER_VERSION}/zsh-in-docker.sh)" -- \
     -p git \
     -p fzf \
     -a "source /usr/share/doc/fzf/examples/key-bindings.zsh" \
     -a "source /usr/share/doc/fzf/examples/completion.zsh" \
     -a "export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
     -x

   # Install Claude
   RUN npm install -g @anthropic-ai/claude-code@${CLAUDE_CODE_VERSION}

   # Copy and set up firewall script
   COPY init-firewall.sh /usr/local/bin/
   USER root
   RUN chmod +x /usr/local/bin/init-firewall.sh && \
     echo "node ALL=(root) NOPASSWD: /usr/local/bin/init-firewall.sh" > /etc/sudoers.d/node-firewall && \
     chmod 0440 /etc/sudoers.d/node-firewall
   USER node
   EOF
   ```
   
   **建立 init-firewall.sh：**
   ```bash
   cat > .devcontainer/init-firewall.sh << 'EOF'
   #!/bin/bash
   set -euo pipefail  # Exit on error, undefined vars, and pipeline failures
   IFS=$'\n\t'       # Stricter word splitting

   # 1. Extract Docker DNS info BEFORE any flushing
   DOCKER_DNS_RULES=$(iptables-save -t nat | grep "127\.0\.0\.11" || true)

   # Flush existing rules and delete existing ipsets
   iptables -F
   iptables -X
   iptables -t nat -F
   iptables -t nat -X
   iptables -t mangle -F
   iptables -t mangle -X
   ipset destroy allowed-domains 2>/dev/null || true

   # 2. Selectively restore ONLY internal Docker DNS resolution
   if [ -n "$DOCKER_DNS_RULES" ]; then
       echo "Restoring Docker DNS rules..."
       iptables -t nat -N DOCKER_OUTPUT 2>/dev/null || true
       iptables -t nat -N DOCKER_POSTROUTING 2>/dev/null || true
       echo "$DOCKER_DNS_RULES" | xargs -L 1 iptables -t nat
   else
       echo "No Docker DNS rules to restore"
   fi

   # First allow DNS and localhost before any restrictions
   # Allow outbound DNS
   iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
   # Allow inbound DNS responses
   iptables -A INPUT -p udp --sport 53 -j ACCEPT
   # Allow outbound SSH
   iptables -A OUTPUT -p tcp --dport 22 -j ACCEPT
   # Allow inbound SSH responses
   iptables -A INPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
   # Allow localhost
   iptables -A INPUT -i lo -j ACCEPT
   iptables -A OUTPUT -o lo -j ACCEPT

   # Create ipset with CIDR support
   ipset create allowed-domains hash:net

   # Fetch GitHub meta information and aggregate + add their IP ranges
   echo "Fetching GitHub IP ranges..."
   gh_ranges=$(curl -s https://api.github.com/meta)
   if [ -z "$gh_ranges" ]; then
       echo "ERROR: Failed to fetch GitHub IP ranges"
       exit 1
   fi

   if ! echo "$gh_ranges" | jq -e '.web and .api and .git' >/dev/null; then
       echo "ERROR: GitHub API response missing required fields"
       exit 1
   fi

   echo "Processing GitHub IPs..."
   while read -r cidr; do
       if [[ ! "$cidr" =~ ^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/[0-9]{1,2}$ ]]; then
           echo "ERROR: Invalid CIDR range from GitHub meta: $cidr"
           exit 1
       fi
       echo "Adding GitHub range $cidr"
       ipset add allowed-domains "$cidr"
   done < <(echo "$gh_ranges" | jq -r '(.web + .api + .git)[]' | aggregate -q)

   # Resolve and add other allowed domains
   for domain in \
       "registry.npmjs.org" \
       "api.anthropic.com" \
       "sentry.io" \
       "statsig.anthropic.com" \
       "statsig.com" \
       "marketplace.visualstudio.com" \
       "vscode.blob.core.windows.net" \
       "update.code.visualstudio.com"; do
       echo "Resolving $domain..."
       ips=$(dig +noall +answer A "$domain" | awk '$4 == "A" {print $5}')
       if [ -z "$ips" ]; then
           echo "ERROR: Failed to resolve $domain"
           exit 1
       fi
       
       while read -r ip; do
           if [[ ! "$ip" =~ ^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}$ ]]; then
               echo "ERROR: Invalid IP from DNS for $domain: $ip"
               exit 1
           fi
           echo "Adding $ip for $domain"
           ipset add allowed-domains "$ip"
       done < <(echo "$ips")
   done

   # Get host IP from default route
   HOST_IP=$(ip route | grep default | cut -d" " -f3)
   if [ -z "$HOST_IP" ]; then
       echo "ERROR: Failed to detect host IP"
       exit 1
   fi

   HOST_NETWORK=$(echo "$HOST_IP" | sed "s/\.[0-9]*$/.0\/24/")
   echo "Host network detected as: $HOST_NETWORK"

   # Set up remaining iptables rules
   iptables -A INPUT -s "$HOST_NETWORK" -j ACCEPT
   iptables -A OUTPUT -d "$HOST_NETWORK" -j ACCEPT

   # Set default policies to DROP first
   iptables -P INPUT DROP
   iptables -P FORWARD DROP
   iptables -P OUTPUT DROP

   # First allow established connections for already approved traffic
   iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
   iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

   # Then allow only specific outbound traffic to allowed domains
   iptables -A OUTPUT -m set --match-set allowed-domains dst -j ACCEPT

   # Explicitly REJECT all other outbound traffic for immediate feedback
   iptables -A OUTPUT -j REJECT --reject-with icmp-admin-prohibited

   echo "Firewall configuration complete"
   echo "Verifying firewall rules..."
   if curl --connect-timeout 5 https://example.com >/dev/null 2>&1; then
       echo "ERROR: Firewall verification failed - was able to reach https://example.com"
       exit 1
   else
       echo "Firewall verification passed - unable to reach https://example.com as expected"
   fi

   # Verify GitHub API access
   if ! curl --connect-timeout 5 https://api.github.com/zen >/dev/null 2>&1; then
       echo "ERROR: Firewall verification failed - unable to reach https://api.github.com"
       exit 1
   else
       echo "Firewall verification passed - able to reach https://api.github.com as expected"
   fi
   EOF
   ```

3. **自訂配置（可選）**
   ```bash
   # 編輯 devcontainer.json 以符合專案需求
   nano .devcontainer/devcontainer.json
   ```

4. **啟動容器並驗證**
   ```bash
   devcontainer up --workspace-folder .
   devcontainer exec --workspace-folder . zsh
   
   # 在容器內驗證 Claude Code CLI
   claude --version
   ```

### 工作流程 3：在 VS Code 中使用 DevContainer

**目標：** 透過 VS Code 的 DevContainer 擴充功能使用容器

**步驟：**

1. **安裝 VS Code 擴充功能**
   - 安裝 "Dev Containers" 擴充功能

2. **開啟專案**
   ```bash
   # 在 VS Code 中開啟當前目錄
   code .
   ```

3. **重新開啟在容器中**
   - 按 `Cmd+Shift+P` (macOS) 或 `Ctrl+Shift+P` (Windows/Linux)
   - 選擇 "Dev Containers: Reopen in Container"
   - VS Code 會自動建構並連接到容器

## DevContainer 配置詳解

### 主要配置檔案

**`.devcontainer/devcontainer.json`** - 主要配置檔案：
- 容器名稱：Claude Code Sandbox
- 基礎映像：Node.js 20
- VS Code 擴充功能：Claude Code、ESLint、Prettier、GitLens
- 網路權限：NET_ADMIN、NET_RAW（用於防火牆設定）
- 持久化儲存：命令歷史和 Claude 配置

**`.devcontainer/Dockerfile`** - 容器映像定義：
- 基於 Node.js 20 官方映像
- 安裝開發工具：Git、Zsh、FZF、Vim、Nano 等
- 安裝 Claude Code CLI
- 設定防火牆工具：iptables、ipset
- 配置 Zsh 和 Powerline10k 主題

**`.devcontainer/init-firewall.sh`** - 防火牆初始化腳本：
- 設定安全的網路存取規則
- 允許特定域名和服務（GitHub、npm、Anthropic API 等）
- 阻擋未授權的外部連線

### 環境變數

| 變數 | 預設值 | 說明 |
|------|--------|------|
| `TZ` | America/Los_Angeles | 時區設定 |
| `CLAUDE_CODE_VERSION` | latest | Claude Code CLI 版本 |
| `NODE_OPTIONS` | --max-old-space-size=4096 | Node.js 記憶體限制 |
| `CLAUDE_CONFIG_DIR` | /home/node/.claude | Claude 配置目錄 |

## 故障排除

### 錯誤：npm 權限被拒絕 (EACCES)

**症狀：**
- 執行 `npm install -g` 時出現 "EACCES: permission denied" 錯誤
- 錯誤訊息提到 `/usr/local/lib/node_modules/` 路徑

**原因：** npm 嘗試安裝到預設的全域目錄，但 `node` 使用者沒有權限

**解決方案：**

1. **確認 npm 配置**
   ```bash
   # 檢查當前的 npm 配置
   npm config get prefix
   echo $NPM_CONFIG_PREFIX
   ```

2. **重新設定 npm 配置（如果需要）**
   ```bash
   # 設定正確的 npm 全域安裝路徑
   export NPM_CONFIG_PREFIX=/usr/local/share/npm-global
   export PATH=$PATH:/usr/local/share/npm-global/bin
   
   # 或者使用 npm config 設定
   npm config set prefix /usr/local/share/npm-global
   ```

3. **重新安裝 Claude Code CLI**
   ```bash
   npm install -g @anthropic-ai/claude-code@latest
   ```

4. **驗證安裝**
   ```bash
   claude --version
   which claude
   ```

**永久解決方案：**
將 npm 配置加入到 shell 配置檔案：
```bash
# 加入到 ~/.zshrc
echo 'export NPM_CONFIG_PREFIX=/usr/local/share/npm-global' >> ~/.zshrc
echo 'export PATH=$PATH:/usr/local/share/npm-global/bin' >> ~/.zshrc
source ~/.zshrc
```

### 錯誤：Claude Code CLI 未安裝或無法執行

**症狀：**
- 在容器內執行 `claude --version` 顯示 "command not found"
- Claude Code CLI 無法正常運作

**可能原因：**
1. 容器建構過程中 npm 安裝失敗
2. PATH 環境變數設定問題
3. 網路連線問題導致套件下載失敗

**解決方案：**

1. **檢查 Claude Code CLI 是否已安裝**
   ```bash
   # 在容器內執行
   npm list -g @anthropic-ai/claude-code
   ```

2. **手動重新安裝 Claude Code CLI**
   ```bash
   # 在容器內執行 - 使用正確的 npm 配置
   npm install -g @anthropic-ai/claude-code@latest
   
   # 如果出現權限錯誤，確認 npm 配置：
   echo $NPM_CONFIG_PREFIX
   # 應該顯示：/usr/local/share/npm-global
   
   # 如果 NPM_CONFIG_PREFIX 未設定，手動設定：
   export NPM_CONFIG_PREFIX=/usr/local/share/npm-global
   export PATH=$PATH:/usr/local/share/npm-global/bin
   npm install -g @anthropic-ai/claude-code@latest
   ```

3. **檢查 npm 配置和權限**
   ```bash
   # 確認 npm 全域安裝路徑配置
   echo $NPM_CONFIG_PREFIX
   echo $PATH
   
   # 檢查目錄權限
   ls -la /usr/local/share/npm-global
   
   # 如果目錄不存在或權限不正確，重新設定：
   mkdir -p /usr/local/share/npm-global
   # 注意：在容器內 node 使用者應該已經有權限
   ```

4. **重新建構容器（如果問題持續）**
   ```bash
   # 在主機上執行，強制重新建構
   devcontainer up --workspace-folder . --remove-existing-container --no-cache
   ```

5. **檢查容器建構日誌**
   ```bash
   # 查看建構過程中的錯誤訊息
   docker logs <container-id>
   ```

**驗證修復：**
```bash
# 在容器內執行這些指令確認 Claude Code CLI 正常運作
claude --version
claude --help
```

### 錯誤：找不到模板檔案

**原因：** 此 Power 不包含模板檔案，所有配置內容都在 POWER.md 中
**解決方案：**
1. 使用「工作流程 2」中提供的完整檔案建立指令
2. 直接複製 POWER.md 中的配置內容來建立檔案
3. 確保按照指令順序建立所有三個檔案：devcontainer.json、Dockerfile、init-firewall.sh

### 錯誤：找不到 devcontainer.json

**原因：** 在錯誤的目錄執行指令，或 `.devcontainer` 目錄結構不正確
**解決方案：**
1. 確認目前目錄是專案根目錄：
   ```bash
   pwd
   ls -la .devcontainer/
   ```
2. 確認 `.devcontainer` 目錄直接在專案根目錄下（不是在子目錄中）
3. 確認 `devcontainer.json` 檔案存在：
   ```bash
   ls -la .devcontainer/devcontainer.json
   ```
4. 如果目錄結構錯誤，重新建立正確的結構

### 錯誤：DevContainer CLI 未找到

**原因：** DevContainer CLI 未正確安裝或不在 PATH 中
**解決方案：**
1. 重新安裝 DevContainer CLI：
   ```bash
   npm uninstall -g @devcontainers/cli
   npm install -g @devcontainers/cli
   ```
2. 驗證安裝：`devcontainer --version`
3. 重新啟動終端機

### 錯誤：Docker 連線失敗

**原因：** Docker 服務未運行或權限不足
**解決方案：**
1. 確認 Docker 運行狀態：
   ```bash
   docker ps
   ```
2. 啟動 Docker Desktop（macOS/Windows）或 Docker 服務（Linux）
3. 檢查 Docker 權限設定

### 錯誤：容器建構失敗

**症狀：**
- 建構過程中出現網路錯誤
- 套件安裝失敗
- 權限拒絕錯誤

**解決方案：**
1. 清理 Docker 快取：
   ```bash
   docker system prune -a
   ```
2. 重新建構容器：
   ```bash
   devcontainer up --workspace-folder . --remove-existing-container
   ```
3. 檢查網路連線和防火牆設定

### 錯誤：防火牆設定失敗

**原因：** 容器缺少網路管理權限
**解決方案：**
1. 確認 `devcontainer.json` 中包含必要的權限：
   ```json
   "runArgs": [
     "--cap-add=NET_ADMIN",
     "--cap-add=NET_RAW"
   ]
   ```
2. 重新啟動容器
3. 檢查 `init-firewall.sh` 腳本權限

### 錯誤：VS Code 擴充功能未載入

**原因：** 擴充功能配置錯誤或網路問題
**解決方案：**
1. 檢查 `devcontainer.json` 中的擴充功能列表
2. 手動安裝擴充功能：
   ```bash
   # 在容器內執行
   code --install-extension anthropic.claude-code
   ```
3. 重新載入 VS Code 視窗

## 最佳實踐

### 專案組織
- **將 `.devcontainer` 目錄放在專案根目錄**（不是子目錄）
- 使用版本控制追蹤 DevContainer 配置
- 為不同專案類型建立不同的 DevContainer 模板
- 確保 `devcontainer up` 和 `devcontainer exec` 指令在專案根目錄執行

### 效能優化
- 使用 Docker 卷來持久化資料（命令歷史、配置等）
- 定期清理未使用的 Docker 映像和容器
- 調整 Node.js 記憶體限制以符合系統資源

### 安全考量
- 定期更新基礎映像和套件
- 檢查防火牆規則是否符合安全需求
- 避免在容器中儲存敏感資訊

### 團隊協作
- 統一團隊的 DevContainer 配置
- 在 README 中記錄特定的設定需求
- 使用 `.devcontainer/devcontainer.json` 的註解說明自訂設定

## 進階配置

### 自訂 Dockerfile

如需修改容器環境，編輯 `.devcontainer/Dockerfile`：

```dockerfile
# 新增額外的套件
RUN apt-get update && apt-get install -y \
  your-additional-package \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# 安裝額外的 Node.js 套件
RUN npm install -g your-global-package
```

### 修改防火牆規則

編輯 `.devcontainer/init-firewall.sh` 以新增允許的域名：

```bash
# 在現有域名列表中新增
for domain in \
    "your-custom-domain.com" \
    "another-allowed-site.com"; do
    # ... 解析和新增邏輯
done
```

### VS Code 設定自訂

在 `devcontainer.json` 中新增 VS Code 設定：

```json
{
  "customizations": {
    "vscode": {
      "settings": {
        "your.custom.setting": "value"
      },
      "extensions": [
        "your.custom.extension"
      ]
    }
  }
}
```

## 配置

**無需額外配置** - 使用預設的 DevContainer 配置即可運行。

**可選配置：**
- **時區設定**：修改 `devcontainer.json` 中的 `TZ` 參數
- **Claude Code 版本**：調整 `CLAUDE_CODE_VERSION` 參數
- **記憶體限制**：修改 `NODE_OPTIONS` 環境變數

---

**工具：** DevContainer CLI (`@devcontainers/cli`)
**容器：** Node.js 20 + Claude Code + 開發工具
**配置檔案：** `.devcontainer/devcontainer.json`, `.devcontainer/Dockerfile`, `.devcontainer/init-firewall.sh`