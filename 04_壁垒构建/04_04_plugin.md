---
title: Plugin
description: 理解Plugin概念，掌握插件开发与部署
---

# Plugin

> **模块目标**: 理解Plugin与Skills的区别，掌握Plugin的开发、部署和分享方法

---

## 一、什么是Plugin

### 1.1 Plugin的定义

Plugin（插件）是基于MCP协议的功能扩展，让AI能够执行特定的操作，如：
- 获取外部数据（天气、股票、新闻等）
- 操作系统功能（文件、进程等）
- 调用第三方API
- 执行复杂计算

**与Skills的区别**：
- Skills侧重于**提示词模板**
- Plugin侧重于**功能实现**

### 1.2 Plugin的应用场景

| 场景 | 说明 | 示例 |
|-----|------|------|
| **数据获取** | 从外部API获取数据 | 天气查询、股票行情 |
| **文件操作** | 读写本地文件 | 批量重命名、格式转换 |
| **系统集成** | 与其他软件交互 | 发送邮件、创建日历事件 |
| **数据处理** | 执行复杂计算 | 数据分析、图表生成 |

### 1.3 Plugin的价值

**没有Plugin时**：
```
用户：帮我查一下北京的天气
AI：抱歉，我无法获取实时天气信息...
```

**有了Plugin后**：
```
用户：帮我查一下北京的天气
AI：[调用天气Plugin]
北京今天晴，温度15-25°C，空气质量良好。
```

---

## 二、Plugin与Skills的异同

### 2.1 共同点

| 共同点 | 说明 |
|-------|------|
| **都基于MCP** | 使用相同的协议标准 |
| **扩展AI能力** | 让AI能做更多事情 |
| **可分享复用** | 一次开发，多处使用 |
| **标准化接口** | 遵循统一的开发规范 |

### 2.2 区别

| 对比维度 | Skills | Plugin |
|---------|--------|--------|
| **核心能力** | 提示词模板封装 | 功能代码实现 |
| **开发难度** | 低（主要是配置） | 中（需要编程） |
| **技术要求** | 了解JSON格式 | 了解编程语言 |
| **功能范围** | 文本生成、格式化 | 任何可编程的功能 |
| **外部交互** | 有限 | 完全支持 |

### 2.3 如何选择

**选择Skills的场景**：
- ✅ 主要是文本生成任务
- ✅ 不需要调用外部API
- ✅ 不想写代码
- ✅ 快速实现简单功能

**选择Plugin的场景**：
- ✅ 需要获取外部数据
- ✅ 需要操作文件系统
- ✅ 需要与第三方服务集成
- ✅ 需要执行复杂计算

**关系图**：

```
┌─────────────────────────────────────┐
│             MCP协议                  │
├──────────────────┬──────────────────┤
│     Skills       │     Plugin       │
│  (提示词封装)     │   (功能实现)      │
│  - 文本模板       │   - API调用       │
│  - 格式化         │   - 文件操作      │
│  - 简单任务       │   - 系统集成      │
└──────────────────┴──────────────────┘
```

---

## 三、让AI写一个Plugin

### 3.1 Plugin开发基础

#### 技术栈要求

| 技术 | 说明 | 必要程度 |
|-----|------|---------|
| **JavaScript/TypeScript** | 最常用的Plugin开发语言 | ⭐⭐⭐ |
| **Python** | 也可以用于开发Plugin | ⭐⭐ |
| **MCP SDK** | Anthropic提供的开发工具包 | ⭐⭐⭐ |
| **API调用** | 理解REST API的基本概念 | ⭐⭐ |

#### 开发环境准备

```bash
# 创建项目目录
mkdir my-plugin
cd my-plugin

# 初始化Node.js项目
npm init -y

# 安装MCP SDK
npm install @modelcontextprotocol/sdk
```

### 3.2 Plugin的基本结构

```
my-plugin/
├── package.json      # 项目配置
├── index.js          # 主入口文件
├── README.md         # 说明文档
└── test/             # 测试文件
    └── test.js
```

### 3.3 使用AI辅助开发

**提示词模板**：

```
请帮我创建一个MCP Plugin，具体要求如下：

【功能描述】
[描述Plugin的功能]

【需要的能力】
- [能力1：如调用某个API]
- [能力2：如文件操作]

【输入参数】
- 参数1：[类型] - [说明]
- 参数2：[类型] - [说明]

【输出格式】
[描述期望的输出]

【技术偏好】
- 语言：JavaScript/Python
- 是否需要外部依赖

请生成完整的Plugin代码。
```

### 3.4 实战案例：天气查询Plugin

**第一步：描述需求**

```
请帮我创建一个MCP Plugin，用于查询天气：

【功能描述】
查询指定城市的当前天气信息

【需要的能力】
- 调用天气API获取数据

【输入参数】
- city: string - 城市名称（如"北京"）

【输出格式】
返回天气状况、温度、湿度等信息

【技术偏好】
- 语言：JavaScript
- 使用免费的天气API

请生成完整的Plugin代码。
```

**第二步：AI生成的代码**

**package.json**：
```json
{
  "name": "weather-plugin",
  "version": "1.0.0",
  "description": "MCP Plugin for weather queries",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.0.0"
  }
}
```

**index.js**：
```javascript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import axios from "axios";

// 创建MCP服务器
const server = new Server(
  {
    name: "weather-plugin",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// 定义工具列表
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "get_weather",
        description: "获取指定城市的天气信息",
        inputSchema: {
          type: "object",
          properties: {
            city: {
              type: "string",
              description: "城市名称，如'北京'、'上海'",
            },
          },
          required: ["city"],
        },
      },
    ],
  };
});

// 处理工具调用
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === "get_weather") {
    const { city } = request.params.arguments;
    
    try {
      // 调用天气API（这里使用模拟数据）
      const weather = await getWeather(city);
      
      return {
        content: [
          {
            type: "text",
            text: `${city}的天气：\n` +
                  `- 天气状况：${weather.condition}\n` +
                  `- 温度：${weather.temp}°C\n` +
                  `- 湿度：${weather.humidity}%\n` +
                  `- 风力：${weather.wind}`,
          },
        ],
      };
    } catch (error) {
      return {
        content: [
          {
            type: "text",
            text: `获取天气失败：${error.message}`,
          },
        ],
        isError: true,
      };
    }
  }
  
  throw new Error("Unknown tool");
});

// 天气获取函数
async function getWeather(city) {
  // 实际项目中应该调用真实的天气API
  // 这里使用模拟数据
  const mockData = {
    "北京": { condition: "晴", temp: 22, humidity: 45, wind: "北风3级" },
    "上海": { condition: "多云", temp: 25, humidity: 60, wind: "东南风2级" },
    "广州": { condition: "阴", temp: 28, humidity: 75, wind: "南风2级" },
  };
  
  return mockData[city] || { condition: "晴", temp: 20, humidity: 50, wind: "微风" };
}

// 启动服务器
const transport = new StdServerTransport();
await server.connect(transport);
```

### 3.5 测试Plugin

```bash
# 安装依赖
npm install

# 运行测试
npm test

# 或直接运行
node index.js
```

---

## 四、部署Plugin

### 4.1 本地部署

#### 步骤1：准备Plugin文件

确保Plugin项目结构完整：
```
weather-plugin/
├── package.json
├── index.js
├── node_modules/     # 运行npm install后生成
└── README.md
```

#### 步骤2：配置Claude Desktop

编辑配置文件：
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

添加Plugin配置：
```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/path/to/weather-plugin/index.js"]
    }
  }
}
```

#### 步骤3：重启Claude Desktop

关闭并重新打开Claude Desktop，Plugin即可生效。

### 4.2 服务器部署

如果Plugin需要长期运行或多人使用，可以部署到服务器。

#### 使用Docker部署

**Dockerfile**：
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --production

COPY . .

CMD ["node", "index.js"]
```

**构建和运行**：
```bash
# 构建镜像
docker build -t weather-plugin .

# 运行容器
docker run -d --name weather weather-plugin
```

#### 使用云服务部署

| 平台 | 特点 | 适用场景 |
|-----|------|---------|
| **Vercel** | 简单易用，免费额度 | 轻量级Plugin |
| **Railway** | 支持持久运行 | 需要状态的Plugin |
| **AWS Lambda** | 按需付费，高可用 | 企业级应用 |

### 4.3 调试技巧

#### 查看日志

```bash
# Claude Desktop日志位置
# macOS
tail -f ~/Library/Logs/Claude/mcp*.log

# Windows
type %APPDATA%\Claude\logs\mcp*.log
```

#### 常见问题排查

| 问题 | 可能原因 | 解决方案 |
|-----|---------|---------|
| Plugin未加载 | 配置路径错误 | 检查配置文件中的路径 |
| 调用失败 | 依赖未安装 | 运行`npm install` |
| 超时 | 网络问题或代码卡住 | 检查网络，添加超时处理 |
| 权限错误 | 文件权限不足 | 检查文件执行权限 |

---

## 五、分享你的Plugin

### 5.1 打包与发布

#### 准备发布包

```
weather-plugin/
├── package.json
├── index.js
├── README.md
├── LICENSE
├── .gitignore
└── examples/
    └── basic-usage.md
```

#### README.md模板

```markdown
# Weather Plugin

一个用于查询天气的MCP Plugin。

## 功能

- 查询指定城市的天气信息
- 返回温度、湿度、风力等详细数据

## 安装

### 方法1：npm安装
```bash
npm install weather-plugin
```

### 方法2：手动安装
1. 克隆本仓库
2. 运行`npm install`
3. 配置Claude Desktop...

## 使用方法

在Claude中输入：
```
查询北京的天气
```

## 配置说明

...

## 许可证

MIT
```

### 5.2 发布到npm

```bash
# 登录npm
npm login

# 发布
npm publish

# 发布后的安装命令
npm install your-plugin-name
```

### 5.3 发布到GitHub

```bash
# 初始化Git
git init

# 添加文件
git add .

# 提交
git commit -m "Initial commit"

# 添加远程仓库
git remote add origin https://github.com/username/weather-plugin.git

# 推送
git push -u origin main
```

### 5.4 用户支持

**提供良好的文档**：
- 安装指南
- 使用示例
- 常见问题
- 更新日志

**建立反馈渠道**：
- GitHub Issues
- 讨论区
- 邮件支持

---

## 六、进阶：复杂Plugin开发

### 6.1 多功能Plugin

一个Plugin可以包含多个工具：

```javascript
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "get_weather",
        description: "获取天气",
        inputSchema: { /* ... */ }
      },
      {
        name: "get_forecast",
        description: "获取天气预报",
        inputSchema: { /* ... */ }
      },
      {
        name: "get_air_quality",
        description: "获取空气质量",
        inputSchema: { /* ... */ }
      }
    ],
  };
});
```

### 6.2 带状态的Plugin

```javascript
// 保存会话状态
const sessions = new Map();

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const sessionId = request.params.arguments?.session_id;
  
  // 获取或创建会话
  if (!sessions.has(sessionId)) {
    sessions.set(sessionId, { history: [] });
  }
  
  const session = sessions.get(sessionId);
  session.history.push(request.params);
  
  // 处理请求...
});
```

### 6.3 错误处理最佳实践

```javascript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  try {
    // 参数验证
    if (!request.params.arguments?.city) {
      throw new Error("缺少必需参数：city");
    }
    
    // 执行操作
    const result = await getWeather(request.params.arguments.city);
    
    return {
      content: [{ type: "text", text: formatResult(result) }]
    };
    
  } catch (error) {
    // 分类错误处理
    if (error instanceof NetworkError) {
      return {
        content: [{ type: "text", text: "网络错误，请稍后重试" }],
        isError: true,
      };
    }
    
    if (error instanceof ValidationError) {
      return {
        content: [{ type: "text", text: `参数错误：${error.message}` }],
        isError: true,
      };
    }
    
    // 未知错误
    console.error("Unexpected error:", error);
    return {
      content: [{ type: "text", text: "发生未知错误" }],
      isError: true,
    };
  }
});
```

---

## 七、本章小结

### 核心要点回顾

1. **Plugin是什么**：基于MCP的功能扩展，让AI能执行特定操作
2. **与Skills的区别**：Plugin侧重功能实现，Skills侧重提示词封装
3. **如何开发**：使用JavaScript/Python，借助AI辅助生成代码
4. **如何部署**：本地配置或服务器部署
5. **如何分享**：npm发布或GitHub开源

### 自检清单

完成本章学习后，问问自己：

- [ ] 我理解Plugin和Skills的区别了吗？
- [ ] 我知道什么场景应该用Plugin吗？
- [ ] 我能用AI辅助开发一个简单的Plugin吗？
- [ ] 我知道如何部署和测试Plugin吗？
- [ ] 我了解如何分享我的Plugin吗？

### 下一步

掌握Plugin后，你已准备好进入下一个模块：

**→ [在线工具](04_05_在线工具.md)**

在下一模块中，你将学习Coze等在线平台，无需本地部署也能创建强大的AI应用。

---

*Plugin是扩展AI能力的利器。掌握它，你将能够让AI与外部世界交互，实现真正的智能化应用！*
