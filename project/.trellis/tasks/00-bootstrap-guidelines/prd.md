# 引导：填写项目开发指南

## 目的

欢迎使用 Trellis！这是你的第一个任务。

AI 代理通过 `.trellis/spec/` 了解**你们项目**的编码约定。
**空模板 = AI 写出与项目风格不符的通用代码。**

填写这些指南是一次性投入，会在之后的每次 AI 会话中受益。

---

## 你的任务

根据**现有代码库**填写指南文件。

### 后端指南

| 文件 | 记录内容 |
|------|------------------|
| `.trellis/spec/backend/directory-structure.md` | 各类型文件放在哪里（路由、服务、工具等） |
| `.trellis/spec/backend/database-guidelines.md` | ORM、迁移、查询模式、命名约定 |
| `.trellis/spec/backend/error-handling.md` | 如何捕获、记录并返回错误 |
| `.trellis/spec/backend/logging-guidelines.md` | 日志级别、格式、应记录什么 |
| `.trellis/spec/backend/quality-guidelines.md` | Code Review 标准、测试要求 |

### 前端指南

| 文件 | 记录内容 |
|------|------------------|
| `.trellis/spec/frontend/directory-structure.md` | 组件/页面/hook 的组织方式 |
| `.trellis/spec/frontend/component-guidelines.md` | 组件模式、props 约定 |
| `.trellis/spec/frontend/hook-guidelines.md` | 自定义 hook 命名与模式 |
| `.trellis/spec/frontend/state-management.md` | 状态库、模式、各类状态放哪里 |
| `.trellis/spec/frontend/type-safety.md` | TypeScript 约定、类型组织 |
| `.trellis/spec/frontend/quality-guidelines.md` | Lint、测试、无障碍 |

### 思考指南（可选）

`.trellis/spec/guides/` 目录中的思考指南已填入通用最佳实践。可按项目需要再定制。

---

## 如何填写指南

### 步骤 0：从已有 Spec 导入（推荐）

许多项目已有编码约定文档。**先查这些**，再从零写：

| 文件 / 目录 | 工具 |
|------|------|
| `CLAUDE.md` / `CLAUDE.local.md` | Claude Code |
| `AGENTS.md` | Claude Code |
| `.cursorrules` | Cursor |
| `.cursor/rules/*.mdc` | Cursor（规则目录） |
| `.windsurfrules` | Windsurf |
| `.clinerules` | Cline |
| `.roomodes` | Roo Code |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `.vscode/settings.json` → `github.copilot.chat.codeGeneration.instructions` | VS Code Copilot |
| `CONVENTIONS.md` / `.aider.conf.yml` | aider |
| `CONTRIBUTING.md` | 通用项目约定 |
| `.editorconfig` | 编辑器格式化规则 |

若其中任一项存在，先阅读并把相关编码约定提取到对应的 `.trellis/spec/` 文件中。比从零写要省力得多。

### 步骤 1：分析代码库

可请 AI 从实际代码中发现模式：

- 「阅读所有现有配置文件（CLAUDE.md、.cursorrules 等），把编码约定提取到 .trellis/spec/」
- 「分析我的代码库并记录你看到的模式」
- 「查找错误处理/组件/API 模式并写成文档」

### 步骤 2：记录现实，而非理想

写代码库**实际怎么做**，而不是你希望它怎么做。
AI 需要匹配既有模式，而不是引入新模式。

- **看现有代码**——每种模式找 2–3 个例子
- **包含文件路径**——用真实文件作示例
- **列出反模式**——团队刻意避免什么？

---

## 完成检查清单

- [ ] 已按项目类型填写指南
- [ ] 每个指南至少 2–3 个真实代码示例
- [ ] 已记录反模式

完成后：

- 在当前任务的 `notes.md` 中标记完成状态
- 在当前任务的 `notes.md` 中补充初始化结果、验证和交接信息

---

## 为什么重要

完成本任务后：

1. AI 会按你们项目风格写代码
2. 相关的 `/trellis:before-dev` 命令会注入真实上下文
3. `/trellis:check-dev` 会按你们实际标准校验
4. 未来开发者（人或 AI）能更快上手
