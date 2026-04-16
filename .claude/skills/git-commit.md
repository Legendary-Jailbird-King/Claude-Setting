---
name: git-commit
description: 智能生成符合规范的 Git commit 信息
---

# Git 提交 Skill

帮我创建符合规范的 Git commit 信息和提交。

## Commit 规范

采用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

- **feat**: 新功能
- **fix**: 修复 bug
- **docs**: 文档变更
- **style**: 代码格式（不影响代码运行的变动）
- **refactor**: 重构（既不是新增功能，也不是修改bug的代码变动）
- **perf**: 性能优化
- **test**: 测试相关
- **chore**: 构建过程或辅助工具的变动
- **ci**: CI/CD 配置文件和脚本的变动
- **revert**: 回滚

### Subject 格式

- 使用中文描述
- 以动词开头，使用第一人称现在时
- 第一个字母小写
- 结尾不加句号

## 工作流程

1. **检查变更**
   ```bash
   git status
   git diff
   git diff --staged
   ```

2. **分析代码变更**
   - 确定变更类型（feat/fix/refactor等）
   - 识别影响范围（组件/模块/页面）
   - 总结变更内容

3. **生成 commit 信息**
   - 根据变更生成规范的 commit 信息
   - 确保信息清晰、简洁、准确

4. **执行提交**
   ```bash
   git add <files>
   git commit -m "<commit message>"
   ```

## 输出格式

```markdown
## 📋 变更分析
- 变更类型: {{feat/fix/refactor等}}
- 影响范围: {{组件/模块}}
- 变更文件: {{列出主要变更文件}}

## 📝 Commit 信息

```
{{生成的 commit 信息}}
```

## ✅ 执行结果
{{提交后的反馈}}
```

## Commit 示例

```bash
# 新功能
feat(button): 添加加载状态支持

feat(user): 实现用户登录功能

# Bug 修复
fix: 修复在 Safari 浏览器中的布局问题

fix(auth): 修复 token 过期后的处理逻辑

# 重构
refactor(utils): 优化日期格式化函数性能

refactor: 简化状态管理逻辑

# 文档
docs: 更新 README 安装说明

docs(api): 添加接口文档注释

# 样式
style: 统一代码缩进格式

style(component): 调整组件样式命名

# 性能优化
perf: 减少不必要的重渲染

perf(image): 添加图片懒加载

# 测试
test: 添加单元测试覆盖

test(user): 补充登录流程测试用例

# 构建/工具
chore: 升级依赖包版本

chore: 添加 Prettier 配置

# CI/CD
ci: 添加 GitHub Actions 工作流

ci: 优化构建配置
```

## 多行 Commit 示例

```bash
feat(auth): 添加第三方登录支持

- 新增 Google 登录
- 新增 GitHub 登录
- 添加登录状态持久化

Closes #123
```

## 使用方式

在 Claude 中说：
- "帮我 git-commit"
- "生成 commit 信息"
- "提交代码"
- "用 git-commit 提交这些变更"
