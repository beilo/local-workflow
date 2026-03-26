---
name: start
description: "开始会话"
---

# 开始会话

使用纯文件驱动工作流初始化会话。

## 会话开始

### 1. 阅读工作流与本地规则

请先阅读这些文件：

- `.trellis/workflow.md`

### 2. 阅读项目上下文

运行：

- `git status`
- `git log --oneline -10`

阅读：

- `.trellis/spec/frontend/index.md`
- `.trellis/spec/backend/index.md`
- `.trellis/spec/guides/index.md`

查看 `.trellis/tasks/`，确定用户要处理的任务目录。**`.trellis/archive/` 为已归档目录，跳过，不读取**。

若 `spec/` 仍主要是模板内容，且 `00-bootstrap-guidelines` 未完成，优先把它视为当前任务。

## 若需要新任务

在 `.trellis/tasks/` 下手动新建任务目录。

最少包含：

- `prd.md` → 使用 `assets/prd-template.md`
- `notes.md` → 使用 `assets/notes-template.md`

## 开发流程

### 默认轻量流程

1. 使用 `$before-dev`
2. 实现改动
3. 更新任务的 `notes.md`
4. 使用 `$check-dev`
5. 使用 `$finish-work`
6. 人工测试并提交后，更新 `notes.md` 中的最终状态与交接信息

### 需求较重流程

1. 使用 `$brainstorm`，将需求收敛到 `prd.md`
2. 完成 PRD 自审，并让用户确认书面需求
3. 如需，使用 `$writing-plans` 创建 `plan.md`
4. 再任选其一：
   - 默认轻量单会话实现
   - `$executing-plans`
   - `$subagent-driven-development`（仅在任务可清晰拆分时）

## 可选执行类技能

以下不属于默认工作流：

- `$executing-plans`
- `$subagent-driven-development`

可接受：

- `prd.md`
- `plan.md`
- 用户明确提供的其他任务文档

规则：

- 简单的 `prd.md` 可直接执行
- 复杂的 `prd.md` 应先转为 `plan.md`
- 若文档存在明显缺口、占位符或关键歧义，先回写 `prd.md` / `plan.md`
- 若 `plan.md` 已写完，先确认执行方式，再开始改代码
- 开始改代码前，执行 `before-dev`
- 代码改完后，执行 `check-dev`

## 文件边界

- `prd.md` → 目标、范围、验收标准、约束
- `plan.md` → 执行顺序、涉及文件、验证点、停止条件 / 风险
- `notes.md` → 任务状态、执行方式、来源文档、当前检查点、决策、涉及文件、风险、计划偏差、子代理分工（如适用）、下一步、交接、验证、提交信息
