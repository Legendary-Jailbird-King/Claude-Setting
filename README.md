# Claude Code 配置集

> 超级无敌牛的 Claude Code 使用配置 🚀

这是一个精选的 Claude Code 配置集合，包含自定义 Skills、项目模板和最佳实践，帮助你更高效地使用 Claude AI 进行开发。

## ✨ 特性

- 🎯 **自定义 Skills** - 针对常见开发场景的专用技能
- 📝 **规范模板** - 开箱即用的项目配置模板
- 🔒 **安全配置** - 预设的权限配置，安全可控
- 📚 **完整文档** - 详细的使用说明和最佳实践

## 📦 包含内容

```
Claude-Setting/
├── .claude/
│   ├── skills/              # 自定义 Skills
│   │   ├── git-commit.md   # 智能提交信息生成
│   │   ├── code-review.md  # 代码审查
│   │   ├── write-test.md   # 测试生成
│   │   ├── explain-code.md # 代码解释
│   │   ├── read-docs.md    # 文档阅读
│   │   ├── search-docs.md  # 文档搜索
│   │   └── summarize-docs.md # 文档摘要
│   ├── docs/
│   │   └── SKILLS_GUIDE.md # Skills 使用指南
│   ├── CLAUDE.md           # AI 角色配置
│   ├── CLAUDE.md.template  # 项目模板
│   ├── settings.json       # 权限配置
│   └── settings.json.template
└── README.md
```

## 🚀 快速开始

### 1. 安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

### 2. 复制配置到你的项目

```bash
# 复制整个 .claude 文件夹到你的项目根目录
cp -r Claude-Setting/.claude/ your-project/
```

### 3. 根据需要调整配置

编辑 `.claude/CLAUDE.md` 定义你的项目规范
编辑 `.claude/settings.json` 配置权限

### 4. 开始使用

```bash
cd your-project
claude
```

## 🎯 可用 Skills

### git-commit

智能生成符合 Conventional Commits 规范的提交信息。

```
"帮我 git-commit"
"生成 commit 信息"
```

### code-review

全面审查代码质量、安全性和最佳实践。

```
"帮我 code-review 这段代码"
```

### write-test

为组件和函数生成完整的单元测试。

```
"用 write-test 帮我写测试"
```

更多 Skills 请查看 [Skills 使用指南](.claude/docs/SKILLS_GUIDE.md)。

## 📖 配置说明

### CLAUDE.md

定义 AI 的角色、行为和项目规范：

- AI 角色设定
- 技术栈说明
- 编码规范
- 项目结构
- 开发习惯

### settings.json

配置 Claude Code 的权限：

- 允许的文件操作
- 允许的命令
- 环境变量
- 模型设置

## 🔧 自定义

### 创建自己的 Skill

1. 在 `.claude/skills/` 创建新文件
2. 使用以下模板：

```markdown
---
name: your-skill
description: 简短描述
---

# Skill 标题

详细说明...

## 使用方式

在 Claude 中说：
- "命令一"
```

3. 保存即可使用

## 📚 参考资源

- [Claude Code 官方文档](https://code.anthropic.com/docs)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [项目 Skills 指南](.claude/docs/SKILLS_GUIDE.md)

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可

MIT License

---

**让 Claude Code 成为你的超级开发助手！** 🎉
