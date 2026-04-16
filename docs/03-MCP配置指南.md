# MCP 配置指南

MCP（Model Context Protocol）是 Claude 的插件系统，可以扩展 Claude 的能力。

---

## MCP 是什么？

简单理解：**给 Claude 安装插件**

| | MCP | Skill |
|---|-----|-------|
| 是什么 | 外部服务/插件 | 本地定义的技能 |
| 在哪配置 | settings.json | .claude/skills/*.md |
| 作用 | 扩展能力（读文件、搜代码） | 定义行为（怎么审查、怎么解释） |

---

## 常用 MCP 服务器

### 1. Filesystem MCP - 跨项目搜索

**用途**：在多个项目间搜索代码

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\\\Projects"  // 你的项目路径
      ]
    }
  },
  "permissions": {
    "allow": ["mcp__filesystem__*"]
  }
}
```

**使用场景**：
```
"在别的项目里搜索 useEffect 是怎么用的"
"帮我找找有没有写好的表单组件"
```

---

### 2. GitHub MCP - 查看 GitHub 代码

**用途**：查看 GitHub 仓库、PR、Issue

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "你的 GitHub Token"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__github__*"]
  }
}
```

**获取 GitHub Token**：
1. 访问 https://github.com/settings/tokens
2. 生成新 Token
3. 复制 Token 到配置

**使用场景**：
```
"帮我看看 anthropics/claude-code 仓库的最新提交"
"审查我的 PR #123"
```

---

### 3. Brave Search MCP - 搜索技术资料

**用途**：联网搜索最新技术文档

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "你的 API Key"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__brave-search__*"]
  }
}
```

**获取 API Key**：
1. 访问 https://api.search.brave.com/app/keys
2. 注册免费账号
3. 复制 API Key

**使用场景**：
```
"搜索 React Query 最新文档"
"查一下 Ant Design Table 组件怎么用"
```

---

### 4. Web Reader MCP - 读取网页内容

**用途**：读取网页内容，总结文档

```json
{
  "mcpServers": {
    "web-reader": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

**使用场景**：
```
"总结一下 https://nextjs.org/docs 这页的内容"
"提取这个教程的代码示例"
```

---

## MCP 配置步骤

### Step 1: 选择 MCP

根据你的需求选择要安装的 MCP：

| 需求 | 推荐 MCP |
|------|---------|
| 跨项目搜索代码 | Filesystem |
| 看 GitHub 代码 | GitHub |
| 查技术文档 | Brave Search |
| 读网页内容 | Web Reader |

### Step 2: 配置 settings.json

```json
{
  "mcpServers": {
    "你选择的 MCP": {
      "command": "npx",
      "args": ["包名", "参数"],
      "env": {
        "环境变量": "值"
      }
    }
  },
  "permissions": {
    "allow": ["mcp__你选择的MCP__*"]
  }
}
```

### Step 3: 重启 Claude Code

配置完成后重启使配置生效。

---

## 完整配置示例

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [
      "Read(/**)",
      "Write(./src/**)",
      "Bash(npm *)",
      "WebSearch",
      "mcp__filesystem__*",
      "mcp__github__*"
    ],
    "block": ["Bash(rm -rf *)"]
  },
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "C:\\\\Projects"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

---

## MCP 权限说明

配置 MCP 后，需要在 `permissions.allow` 中添加对应权限：

```json
"permissions": {
  "allow": [
    "mcp__filesystem__*",    // Filesystem MCP 权限
    "mcp__github__*",        // GitHub MCP 权限
    "mcp__brave-search__*"   // Brave Search MCP 权限
  ]
}
```

---

## 故障排查

### MCP 不工作？

1. 检查权限配置是否正确
2. 检查 API Key 是否有效
3. 重启 Claude Code

### 权限被拒绝？

检查 `permissions.allow` 是否包含对应的 MCP 权限：
```json
"allow": ["mcp__你的MCP名称__*"]
```

---

**下一步**: 阅读 [Skill 创建指南](./04-Skill创建指南.md) 了解如何创建自定义 Skill。
