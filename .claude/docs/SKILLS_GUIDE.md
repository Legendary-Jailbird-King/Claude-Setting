# Claude Code Skills 配置指南

本项目包含自定义的 Claude Code Skills，帮助你更高效地进行代码开发、审查和提交。

## 📁 目录结构

```
.claude/
├── skills/              # Skills 定义
│   ├── git-commit.md   # Git 提交 skill
│   ├── code-review.md  # 代码审查 skill
│   ├── write-test.md   # 测试生成 skill
│   └── ...
├── docs/               # 文档
│   └── SKILLS_GUIDE.md # 本文档
├── CLAUDE.md           # 项目配置
└── settings.json       # 权限配置
```

## 🚀 可用 Skills

### 1. git-commit - 智能提交

生成符合 Conventional Commits 规范的 Git 提交信息。

**使用方式：**
```
"帮我 git-commit"
"生成 commit 信息"
"提交代码"
```

**支持的提交类型：**
- `feat` - 新功能
- `fix` - Bug 修复
- `docs` - 文档变更
- `style` - 代码格式
- `refactor` - 重构
- `perf` - 性能优化
- `test` - 测试相关
- `chore` - 构建/工具变动
- `ci` - CI/CD 配置
- `revert` - 回滚

### 2. code-review - 代码审查

检查代码质量、安全性和最佳实践。

**使用方式：**
```
"帮我 code-review 这段代码"
"检查这段代码有没有问题"
```

### 3. write-test - 测试生成

为组件和函数生成完整的单元测试。

**使用方式：**
```
"用 write-test 帮我写测试"
"为这个组件生成测试"
"写单元测试"
```

### 4. explain-code - 代码解释

解释复杂代码的工作原理。

### 5. read-docs - 文档阅读

快速阅读和理解项目文档。

### 6. search-docs - 文档搜索

在文档中搜索特定内容。

### 7. summarize-docs - 文档摘要

生成文档的摘要总结。

## ⚙️ 配置说明

### settings.json

权限配置示例：

```json
{
  "permissions": {
    "allow": [
      "Read(/**)",
      "Write(./src/**)",
      "Bash(npm *)",
      "Bash(git status)",
      "Bash(git diff)",
      "Bash(git add)",
      "Bash(git commit)",
      "WebSearch"
    ],
    "block": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)"
    ]
  }
}
```

**重要提示：** 使用 `git-commit` skill 前请确保已允许相关 git 命令权限。

### CLAUDE.md

项目级别的 AI 助手配置，定义：
- AI 角色和专长
- 项目技术栈
- 编码规范
- 项目结构

## 📝 创建自定义 Skill

### Skill 模板

```markdown
---
name: your-skill-name
description: 你的 skill 简短描述
---

# Skill 标题

你的 skill 详细说明。

## 工作流程

1. 步骤一
2. 步骤二
3. 步骤三

## 输出格式

```markdown
## 标题
{{内容}}
```

## 使用方式

在 Claude 中说：
- "命令一"
- "命令二"
```

### 命名规范

- 文件名：`kebab-case`（如 `git-commit.md`）
- name 字段：`kebab-case`（如 `git-commit`）
- 描述：简洁清晰，一句话说明功能

## 🔧 故障排除

### Skill 无法识别

1. 检查文件是否在 `.claude/skills/` 目录下
2. 确认 frontmatter 格式正确
3. 确保 name 字段与文件名一致

### Git 命令被阻止

在 `settings.json` 中添加相应权限：

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)"
    ]
  }
}
```

## 📚 参考资源

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Claude Code Documentation](https://code.anthropic.com/docs)

## 🤝 贡献

欢迎提交新的 Skills 和改进建议！

## 📄 许可

MIT License
