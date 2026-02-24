---
title: Claude Code入门
description: 掌握Claude Code的安装配置，理解其在AI工作流中的核心价值
---

# Claude Code入门

> **模块目标**: 掌握Claude Code的安装配置和使用方法，为后续的Skills和Plugin开发打下基础

---

## 一、Claude Code是什么

### 1.1 Claude Code的定义

Claude Code是Anthropic公司官方推出的AI编程助手工具。它不仅仅是一个代码生成器，更是一个能够理解你的项目、帮你管理文件、执行命令的智能助手。

**核心特点**：

| 特点 | 说明 |
|-----|------|
| **官方出品** | Anthropic官方开发，与Claude模型深度集成 |
| **命令行交互** | 在终端中使用，适合开发者工作流 |
| **文件操作** | 能够读取、创建、修改本地文件 |
| **上下文理解** | 理解项目结构，提供连贯的协助 |

### 1.2 Claude Code与Claude网页版的区别

很多人会问：我可以用Claude网页版，为什么还需要Claude Code？

| 对比维度 | Claude网页版 | Claude Code |
|---------|-------------|-------------|
| **文件操作** | 需要手动复制粘贴 | 直接读写本地文件 |
| **项目理解** | 每次对话独立 | 能理解整个项目结构 |
| **命令执行** | 无法执行 | 可以运行命令行指令 |
| **使用场景** | 单次问答、简单任务 | 项目开发、文件管理 |
| **API调用** | 不需要 | 需要配置API Key |

### 1.3 Claude Code vs 其他AI编程工具

| 工具名称 | 特点 | 适用场景 |
|---------|------|---------|
| **Claude Code** | 官方出品、命令行交互、文件操作 | 项目开发、知识管理 |
| **Cursor** | IDE集成、代码补全 | 代码编写 |
| **GitHub Copilot** | 代码补全、IDE集成 | 代码编写 |
| **通义灵码** | 国产、中文友好 | 代码编写 |

**Claude Code的独特优势**：
- ✅ 能够理解和操作整个项目
- ✅ 不仅能写代码，还能管理文档、配置文件
- ✅ 可以执行命令行操作
- ✅ 与Claude模型的强大推理能力结合

---

## 二、Claude Code为什么必要

### 2.1 基本代码撰写

Claude Code最基础的能力是帮助你撰写代码。

**支持的编程语言**：
- Python、JavaScript、TypeScript
- Java、C++、Go、Rust
- HTML、CSS、SQL
- Shell脚本、批处理脚本
- 以及更多...

**使用示例**：

```
你：帮我写一个Python脚本，批量重命名文件夹中的图片

Claude Code：我来帮你创建这个脚本...
[创建 rename_images.py]
[解释代码逻辑]
[询问是否需要运行测试]
```

### 2.2 管理本地文件（不止于写代码）

Claude Code的一个重要价值是**本地文件管理**，特别是Markdown文件的管理。

#### 为什么这很重要？

在AI时代，知识管理的最佳实践是：
1. 用Markdown格式记录知识
2. 让AI能够读取和理解这些文件
3. 让AI帮助你整理、检索、更新知识

**实际应用场景**：

| 场景 | 传统方式 | 使用Claude Code |
|-----|---------|----------------|
| 整理笔记 | 手动打开文件、编辑 | 让AI自动整理格式 |
| 更新文档 | 逐个文件修改 | 批量更新多个文件 |
| 知识检索 | 手动搜索关键词 | AI理解语义后检索 |
| 内容生成 | 复制粘贴到网页版 | 直接在本地生成文件 |

#### 作为本地知识管理工具

```
你：帮我把这个文件夹下的所有Markdown文件整理一下，统一标题格式

Claude Code：我来扫描这个文件夹...
[读取所有.md文件]
[分析标题格式]
[批量修改]
[输出修改报告]
```

### 2.3 与Obsidian等工具的协同

Claude Code可以与你现有的知识管理工具完美配合：

```
你的工作流：
Obsidian（编辑查看） ←→ Markdown文件 ←→ Claude Code（AI处理）
```

**协同方式**：
- 在Obsidian中查看和编辑Markdown文件
- 用Claude Code进行批量处理、格式转换、内容生成
- 两边修改的内容实时同步

---

## 三、本地安装Claude Code

### 3.1 系统要求

| 系统 | 支持情况 | 备注 |
|-----|---------|------|
| Windows | ✅ 支持 | 需要WSL或PowerShell |
| macOS | ✅ 支持 | 原生支持 |
| Linux | ✅ 支持 | 原生支持 |

**前置条件**：
- Node.js 18.0或更高版本
- npm（Node.js自带）
- 稳定的网络连接

### 3.2 安装步骤

#### 步骤1：安装Node.js

**Windows**：
1. 访问 https://nodejs.org/
2. 下载LTS版本安装包
3. 运行安装程序，按提示完成安装

**macOS**：
```bash
# 使用Homebrew安装
brew install node
```

**Linux (Ubuntu/Debian)**：
```bash
# 使用apt安装
sudo apt update
sudo apt install nodejs npm
```

#### 步骤2：验证Node.js安装

```bash
node --version
# 应输出: v18.x.x 或更高

npm --version
# 应输出: 9.x.x 或更高
```

#### 步骤3：安装Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

#### 步骤4：验证安装

```bash
claude --version
# 应输出版本号
```

### 3.3 常见安装问题

#### 问题1：网络超时

**症状**：安装过程中出现网络错误

**解决方案**：
```bash
# 使用国内镜像
npm config set registry https://registry.npmmirror.com
npm install -g @anthropic-ai/claude-code
```

#### 问题2：权限不足

**症状**：提示权限错误（macOS/Linux）

**解决方案**：
```bash
# 使用sudo
sudo npm install -g @anthropic-ai/claude-code
```

#### 问题3：命令未找到

**症状**：安装成功但输入`claude`提示找不到命令

**解决方案**：
- Windows：重启终端或重新打开命令提示符
- macOS/Linux：检查PATH环境变量是否包含npm全局安装路径

---

## 四、使用自己的AI API

Claude Code需要Anthropic的API Key才能使用。本节介绍如何获取和配置。

### 4.1 如何获取API Key

#### 步骤1：注册Anthropic账号

1. 访问 https://console.anthropic.com/
2. 点击"Sign Up"注册账号
3. 完成邮箱验证

#### 步骤2：创建API Key

1. 登录后进入Console
2. 点击"API Keys"
3. 点击"Create Key"
4. 复制生成的API Key（只显示一次，务必保存）

> ⚠️ **重要提示**：API Key类似于密码，不要分享给他人，不要提交到公开代码仓库。

#### 国内用户注意事项

如果你在国内，可能需要：
- 使用稳定的网络环境
- 考虑使用API代理服务
- 或者使用第三方平台提供的Claude API（如OpenRouter）

### 4.2 如何选择模型

Anthropic提供多个Claude模型，各有特点：

| 模型 | 特点 | 适用场景 | 价格 |
|-----|------|---------|------|
| **Claude 3.5 Sonnet** | 综合能力最强，速度快 | 日常开发、复杂任务 | 中等 |
| **Claude 3 Opus** | 推理能力最强 | 复杂分析、深度思考 | 较高 |
| **Claude 3 Haiku** | 速度最快，成本最低 | 简单任务、大量调用 | 最低 |

**选择建议**：
- 🎯 **推荐**：Claude 3.5 Sonnet——综合性能最佳
- 💰 预算有限：Claude 3 Haiku
- 🧠 需要深度推理：Claude 3 Opus

### 4.3 如何配置

#### 方法1：环境变量（推荐）

**Windows (PowerShell)**：
```powershell
# 临时设置（当前会话有效）
$env:ANTHROPIC_API_KEY="your-api-key-here"

# 永久设置
[Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "your-api-key-here", "User")
```

**macOS/Linux**：
```bash
# 临时设置（当前会话有效）
export ANTHROPIC_API_KEY="your-api-key-here"

# 永久设置（添加到~/.bashrc或~/.zshrc）
echo 'export ANTHROPIC_API_KEY="your-api-key-here"' >> ~/.bashrc
source ~/.bashrc
```

#### 方法2：配置文件

创建配置文件 `~/.claude/config.json`：

```json
{
  "api_key": "your-api-key-here",
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 4096
}
```

#### 方法3：首次运行时输入

第一次运行`claude`命令时，会提示输入API Key。

### 4.4 验证配置

```bash
# 启动Claude Code
claude

# 尝试一个简单问题
你：你好，请介绍一下你自己

# 如果收到正常回复，说明配置成功
```

---

## 五、基本使用指南

### 5.1 启动Claude Code

```bash
# 在当前目录启动
claude

# 在指定目录启动
claude /path/to/your/project
```

### 5.2 常用命令

| 命令 | 说明 |
|-----|------|
| `claude` | 启动交互式会话 |
| `claude --help` | 查看帮助信息 |
| `claude --version` | 查看版本号 |
| `claude "你的问题"` | 单次问答模式 |

### 5.3 使用技巧

#### 技巧1：明确指定文件

```
你：请阅读 README.md 文件，然后帮我写一个改进建议
```

#### 技巧2：让Claude Code理解项目结构

```
你：请先浏览一下这个项目的结构，然后告诉我这个项目是做什么的
```

#### 技巧3：批量操作

```
你：帮我把docs文件夹下所有的.md文件的标题都加上序号
```

#### 技巧4：代码审查

```
你：请审查 src/main.py 文件，指出可能的改进点
```

---

## 六、本章小结

### 核心要点回顾

1. **Claude Code是什么**：Anthropic官方的AI编程助手，能操作文件、执行命令
2. **为什么需要**：不仅能写代码，还能管理本地文件，是AI知识管理的利器
3. **如何安装**：通过npm安装，需要Node.js环境
4. **API配置**：获取Anthropic API Key并配置环境变量

### 自检清单

完成本章学习后，问问自己：

- [ ] 我理解Claude Code与网页版的区别了吗？
- [ ] 我成功安装了Claude Code吗？
- [ ] 我获取并配置了API Key吗？
- [ ] 我能用Claude Code进行基本的文件操作吗？
- [ ] 我知道如何选择合适的模型吗？

### 下一步

掌握Claude Code后，你已准备好进入下一个模块：

**→ [Markdown全入门](04_02_markdown全入门.md)**

在下一模块中，你将学习Markdown这一AI友好的文档格式，为知识管理打下基础。

---

*Claude Code是构建AI壁垒的第一步。掌握它，你将拥有一个强大的AI助手，能够帮你处理代码、文档和各种自动化任务！*
