# 企业级 MCP 配置指南

企业通常会配备内部的文档系统、知识库、工单系统等。本指南教你如何配置 MCP 连接这些系统。

---

## 什么是企业级 MCP？

企业级 MCP 是连接公司内部系统的插件，让 Claude 能够访问：
- 内部知识库 / Wiki
- 文档管理系统
- 工单系统 (Jira / 飞书)
- 代码仓库
- CI/CD 系统

---

## 常见企业 MCP 场景

### 场景 1: 内部知识库

**用途**：搜索公司内部文档、规范、FAQ

**常见的内部系统**：
- Confluence
- 飞书文档
- 钉钉文档
- 语雀
- 石墨文档
- 自建 Wiki

### 场景 2: 工单系统

**用途**：查看需求、Bug、任务

**常见系统**：
- Jira
- 飞书项目
- 钉钉项目
- Tapd
- 禅道
- 自建工单系统

### 场景 3: 代码仓库

**用途**：查看代码、PR、审查

**常见系统**：
- GitHub Enterprise
- GitLab
- Gitee
- 自建 Git 系统

---

## 配置方式

### 方式一：使用官方 MCP

如果系统有官方 MCP 支持，直接配置：

```json
{
  "mcpServers": {
    "jira": {
      "command": "npx",
      "args": ["@company/mcp-server-jira"],
      "env": {
        "JIRA_URL": "https://jira.company.com",
        "JIRA_EMAIL": "your-email@company.com",
        "JIRA_API_TOKEN": "your-api-token"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__jira__*"]
  }
}
```

### 方式二：开发自定义 MCP

如果系统没有官方支持，需要开发自定义 MCP：

```typescript
// 公司内部 Wiki MCP 示例
import { Server } from '@modelcontextprotocol/sdk/server/index.js'

const server = new Server({
  name: 'company-wiki',
  version: '1.0.0'
})

// 添加搜索工具
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  if (request.params.name === 'search-wiki') {
    const query = request.params.arguments?.query
    // 调用公司 Wiki API
    const results = await searchCompanyWiki(query)
    return {
      content: [{
        type: 'text',
        text: JSON.stringify(results, null, 2)
      }]
    }
  }
})
```

---

## 常见企业 MCP 配置

### 1. Confluence MCP

```json
{
  "mcpServers": {
    "confluence": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-confluence"],
      "env": {
        "CONFLUENCE_URL": "https://confluence.company.com",
        "CONFLUENCE_USERNAME": "your-username",
        "CONFLUENCE_API_TOKEN": "your-api-token"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__confluence__*"]
  }
}
```

**使用场景**：
```
搜索 Confluence 上的前端规范
查找 Confluence 上的 API 文档
```

---

### 2. 飞书 MCP

```json
{
  "mcpServers": {
    "feishu": {
      "command": "node",
      "args": ["./mcp-servers/feishu-server/dist/index.js"],
      "env": {
        "FEISHU_APP_ID": "your-app-id",
        "FEISHU_APP_SECRET": "your-app-secret"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__feishu__*"]
  }
}
```

**使用场景**：
```
查看飞书文档上的需求
搜索飞书知识库
获取飞书项目任务
```

---

### 3. Jira MCP

```json
{
  "mcpServers": {
    "jira": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-jira"],
      "env": {
        "JIRA_URL": "https://jira.company.com",
        "JIRA_EMAIL": "your-email@company.com",
        "JIRA_API_TOKEN": "your-api-token"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__jira__*"]
  }
}
```

**使用场景**：
```
查看 Jira 需求详情
搜索相关的 Jira 任务
更新 Jira 任务状态
```

---

### 4. GitLab MCP

```json
{
  "mcpServers": {
    "gitlab": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-gitlab"],
      "env": {
        "GITLAB_URL": "https://gitlab.company.com",
        "GITLAB_TOKEN": "your-personal-access-token"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__gitlab__*"]
  }
}
```

**使用场景**：
```
查看 GitLab 上的代码
审查 Merge Request
搜索项目中的代码示例
```

---

## 敏感信息处理

### 不要硬编码敏感信息

❌ 错误做法：
```json
{
  "env": {
    "API_TOKEN": "sk-ant-api03-1234567890"
  }
}
```

✅ 正确做法：
```json
{
  "env": {
    "API_TOKEN": "${API_TOKEN}"  // 使用环境变量
  }
}
```

### 使用环境变量

```bash
# Windows (PowerShell)
$env:API_TOKEN = "your-token"

# macOS/Linux
export API_TOKEN="your-token"
```

### 公司密钥管理

如果公司有密钥管理系统（如 Vault），从密钥系统获取：

```bash
# 从 Vault 获取
vault kv get -field=token secret/claude > .claude/.env
```

---

## 权限配置

### 最小权限原则

只给 Claude 需要的权限：

```json
"permissions": {
  "allow": [
    "mcp__wiki__search",       // 只允许搜索
    "mcp__wiki__read",         // 只允许读取
    // 不包含 write、delete 等危险操作
  ]
}
```

### 只读权限

对于文档类 MCP，建议只配置只读权限：

```json
"permissions": {
  "allow": [
    "mcp__confluence__page:get",
    "mcp__confluence__blog:get",
    "mcp__confluence__search"
  ]
}
```

---

## 完整配置示例

### 企业前端团队配置

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [
      "Read(/**)",
      "Write(./src/**)",
      "Bash(npm *)",
      "WebSearch",
      "mcp__wiki__search",
      "mcp__wiki__read",
      "mcp__jira__read",
      "mcp__gitlab__read",
      "mcp__gitlab__mr:read"
    ],
    "block": [
      "Bash(rm -rf *)",
      "mcp__wiki__write",
      "mcp__wiki__delete",
      "mcp__jira__write",
      "mcp__jira__delete"
    ]
  },
  "mcpServers": {
    "wiki": {
      "command": "node",
      "args": ["./mcp-servers/company-wiki/dist/index.js"],
      "env": {
        "WIKI_API_KEY": "${WIKI_API_KEY}",
        "WIKI_BASE_URL": "${WIKI_BASE_URL}"
      }
    },
    "jira": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-jira"],
      "env": {
        "JIRA_URL": "${JIRA_URL}",
        "JIRA_EMAIL": "${JIRA_EMAIL}",
        "JIRA_API_TOKEN": "${JIRA_API_TOKEN}"
      }
    },
    "gitlab": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-gitlab"],
      "env": {
        "GITLAB_URL": "${GITLAB_URL}",
        "GITLAB_TOKEN": "${GITLAB_TOKEN}"
      }
    }
  }
}
```

---

## 开发自定义 MCP

### 基础结构

```typescript
// mcp-servers/company-wiki/src/index.ts
import { Server } from '@modelcontextprotocol/sdk/server/index.js'

const server = new Server({
  name: 'company-wiki',
  version: '1.0.0'
})

// 定义可用工具
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: 'search-wiki',
        description: '搜索公司 Wiki',
        inputSchema: {
          type: 'object',
          properties: {
            query: { type: 'string', description: '搜索关键词' }
          },
          required: ['query']
        }
      },
      {
        name: 'get-page',
        description: '获取 Wiki 页面内容',
        inputSchema: {
          type: 'object',
          properties: {
            pageId: { type: 'string', description: '页面 ID' }
          },
          required: ['pageId']
        }
      }
    ]
  }
})

// 处理工具调用
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params

  if (name === 'search-wiki') {
    const results = await searchWiki(args.query)
    return {
      content: [{
        type: 'text',
        text: JSON.stringify(results, null, 2)
      }]
    }
  }

  if (name === 'get-page') {
    const page = await getWikiPage(args.pageId)
    return {
      content: [{
        type: 'text',
        text: page.content
      }]
    }
  }

  throw new Error(`Unknown tool: ${name}`)
})
```

### 发布到 npm

```bash
# 在公司内部 npm 发布
npm publish --registry=https://npm.company.com

# 在 settings.json 中使用
{
  "mcpServers": {
    "company-wiki": {
      "command": "npx",
      "args": ["@company/mcp-server-wiki"]
    }
  }
}
```

---

## 使用场景示例

### 场景 1: 需求开发

```
你: 从 Jira 获取 FE-1234 需求详情
Claude: [读取 Jira 需求]

你: 搜索公司 Wiki 上关于用户认证的文档
Claude: [搜索相关文档]

你: 根据需求和文档，生成代码
Claude: [生成符合规范的代码]
```

### 场景 2: 代码审查

```
你: 审查 GitLab 上的 MR !456
Claude: [读取 MR 代码]

你: 参考 Wiki 上的代码规范
Claude: [对照规范给出审查意见]
```

### 场景 3: 问题排查

```
你: 搜索相关 Jira 任务
Claude: [查找类似问题]

你: 查看 Wiki 上的解决方案
Claude: [获取解决方案文档]

你: 给出修复建议
Claude: [生成修复代码]
```

---

## 安全注意事项

### 1. 权限最小化

只给必要的权限，特别是：
- ❌ 不给写入权限
- ❌ 不给删除权限
- ❌ 不给管理权限

### 2. 审计日志

记录 Claude 的操作：

```typescript
// 在 MCP 中添加日志
const logger = {
  search: (query, user) => {
    console.log(`[${new Date().toISOString()}] ${user} searched: ${query}`)
  }
}
```

### 3. 敏感信息过滤

在 MCP 中过滤敏感信息：

```typescript
function sanitizeResponse(data: any): any {
  const sensitiveFields = ['password', 'token', 'secret']
  // 移除敏感字段
  return removeFields(data, sensitiveFields)
}
```

---

## 故障排查

### 问题 1: MCP 连接失败

检查：
1. API 地址是否正确
2. API Token 是否有效
3. 网络连接是否正常
4. 防火墙是否阻止

### 问题 2: 权限被拒绝

检查：
1. `permissions.allow` 是否包含对应 MCP 权限
2. 环境变量是否正确设置
3. Token 是否有足够权限

### 问题 3: 返回数据格式错误

检查：
1. MCP 返回的 JSON 格式是否正确
2. 编码是否为 UTF-8
3. 特殊字符是否正确转义

---

**下一步**: 阅读 [Skill 创建指南](./04-Skill创建指南.md) 了解如何配合 MCP 使用 Skills。
