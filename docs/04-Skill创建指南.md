# Skill 创建指南

Skill 是封装好的"技能模板"，让 Claude 按照特定方式完成任务。

---

## Skill 是什么？

### Skill vs MCP

| | Skill | MCP |
|---|-------|-----|
| 是什么 | 本地定义的技能 | 外部服务/插件 |
| 在哪 | .claude/skills/*.md | settings.json |
| 作用 | 定义行为（怎么做） | 扩展能力（能做什么）|
| 示例 | 代码审查、解释代码 | 读文件、搜代码 |

### Skill 的价值

- 📋 **标准化**：每次都以相同方式完成任务
- 🔄 **可复用**：一次定义，多次使用
- 🎯 **专注**：让 Claude 按你的要求工作

---

## Skill 文件结构

```markdown
---
name: skill名称
description: 技能描述
---

# Skill 标题

你是...（角色定义）

## 工作流程
1. 第一步
2. 第二步
3. 第三步

## 输出格式
{{期望的输出格式}}

## 使用方式
在 Claude 中说：...
```

---

## 创建 Skill 的步骤

### Step 1: 确定 Skill 目的

想清楚这个 Skill 要解决什么问题：

| 需求 | Skill 名称 |
|------|-----------|
| 检查代码问题 | code-review |
| 解释代码 | explain-code |
| 写测试 | write-test |
| 生成文档 | doc-generator |

### Step 2: 创建 Skill 文件

在 `.claude/skills/` 目录下创建文件：

```bash
.claude/skills/你的技能名.md
```

### Step 3: 编写 Skill 内容

使用以下模板：

```markdown
---
name: 你的技能名
description: 一句话描述
---

# 技能标题

你是...（描述 Claude 的角色）

## 任务
{{你要 Claude 做什么}}

## 步骤
1. {{第一步}}
2. {{第二步}}
3. {{第三步}}

## 注意事项
{{需要注意的事项}}

## 输出格式
{{期望的输出格式}}

## 使用方式
在 Claude 中说："..."
```

---

## 常用 Skill 模板

### 模板 1: 代码审查 Skill

```markdown
---
name: code-review
description: 检查代码质量、安全性和最佳实践
---

# 代码审查

你是代码审查专家，帮我检查代码。

## 审查要点
1. 基础检查（语法、类型、lint）
2. 框架规范（React/Vue 规范）
3. 常见错误（边界情况、内存泄漏）
4. 代码质量（可读性、复杂度）

## 输出格式
```
## 🔴 必须修复
...

## 🟡 建议改进
...

## ✅ 做得好的地方
...
```

## 使用方式
"帮我 code-review 这段代码"
```

---

### 模板 2: 代码解释 Skill

```markdown
---
name: explain-code
description: 详细解释代码，帮助理解
---

# 代码解释

你是代码导师，帮我理解代码。

## 解释方式
1. 逐行解释
2. 用简单的语言
3. 用比喻帮助理解
4. 解释为什么这样写

## 输出格式
```
## 这段代码做什么
...

## 逐行解释
...

## 核心概念
...
```

## 使用方式
"用 explain-code 帮我理解"
```

---

### 模板 3: 测试生成 Skill

```markdown
---
name: write-test
description: 生成单元测试
---

# 测试生成

帮我写完整的单元测试。

## 测试框架
Vitest + Testing Library

## 测试覆盖
- 正常情况
- 边界情况
- 错误情况

## 输出格式
```typescript
// 完整可运行的测试代码
```

## 使用方式
"用 write-test 帮我写测试"
```

---

## 使用 Skill

### 方式一：直接说

```
帮我 code-review 这段代码
```

### 方式二：明确指定

```
使用 code-review skill 检查这段代码
```

---

## 高级技巧

### 1. 组合多个 Skill

```
先用 explain-code 解释这段代码，然后用 write-test 写测试
```

### 2. 结合上下文

```
参考 CLAUDE.md 中的规范，用 code-review 检查代码
```

### 3. 自定义输出

```
用 code-review 检查代码，只报告必须修复的问题
```

---

## Skill 最佳实践

### ✅ 好的 Skill

- 目标明确
- 步骤清晰
- 输出格式固定
- 使用示例清楚

### ❌ 不好的 Skill

- 目标模糊
- 步骤混乱
- 没有输出格式
- 没有使用示例

---

## 示例：创建一个"重构代码" Skill

```markdown
---
name: refactor
description: 重构代码，提高质量和可维护性
---

# 代码重构

你是代码重构专家，帮我优化代码。

## 重构原则
1. 保持功能不变
2. 提高可读性
3. 减少复杂度
4. 提高复用性

## 重构步骤
1. 分析代码问题
2. 给出重构方案
3. 写出重构后的代码
4. 解释改动点

## 输出格式
```
## 发现的问题
...

## 重构方案
...

## 重构后的代码
```typescript
// ...
```

## 改动说明
...
```

## 使用方式
"用 refactor 帮我优化这段代码"
```

---

## 目录结构

```
.claude/
└── skills/
    ├── code-review.md
    ├── explain-code.md
    ├── write-test.md
    ├── refactor.md
    └── doc-generator.md
```

---

**下一步**: 阅读 [常见问题](./05-常见问题.md) 了解更多配置技巧。
