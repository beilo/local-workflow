---
name: trellis-local
description: |
  local-workflow 项目专属的 Trellis 定制说明。
  本技能记录在本项目中对原版 Trellis 所做的修改。
---

# Trellis Local - local-workflow

## 基准版本

Trellis 版本：0.3.7  
初始化日期：2026-03-20

## 定制内容

### 已改动的技能

#### before-dev

- **文件**：`.agents/skills/before-dev/SKILL.md`
- **目的**：将前后端开发前指南阅读合并为一个技能
- **添加日期**：2026-03-20
- **原因**：简化开始编码前的入口

#### check-dev

- **文件**：`.agents/skills/check-dev/SKILL.md`
- **目的**：将前端、后端与跨层核对合并为一个技能
- **添加日期**：2026-03-20
- **原因**：实现后用一套技能做回顾，而非多套检查

#### start

- **文件**：`.agents/skills/start/SKILL.md`
- **改动**：改为纯文件驱动的会话入口技能
- **日期**：2026-03-20
- **原因**：不再依赖 Trellis 脚本与任务编排

#### writing-plans

- **文件**：`.agents/skills/writing-plans/SKILL.md`
- **改动**：本地化，将 `plan.md` 写入当前任务目录
- **日期**：2026-03-21
- **原因**：实施计划保留在 `.trellis/tasks/<task>/` 内

#### executing-plans

- **文件**：`.agents/skills/executing-plans/SKILL.md`
- **改动**：新增为可选的文档驱动执行器
- **日期**：2026-03-21
- **原因**：允许单会话从 `prd.md`、`plan.md` 或其他任务文档执行

#### subagent-driven-development

- **文件**：`.agents/skills/subagent-driven-development/SKILL.md`
- **改动**：新增为可选的文档驱动子代理执行器
- **日期**：2026-03-21
- **原因**：支持高级执行模式，但不纳入默认流程

#### record-session

- **文件**：`.agents/skills/record-session/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-22
- **原因**：不再单独维护会话日志，交接与验证直接收进 `notes.md`

#### onboard

- **文件**：`.agents/skills/onboard/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-20
- **原因**：项目不再需要入职工作流

#### create-command

- **文件**：`.agents/skills/create-command/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-20
- **原因**：项目不再需要技能创建工作流

#### integrate-skill

- **文件**：`.agents/skills/integrate-skill/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-20
- **原因**：项目不再需要技能集成工作流

#### break-loop

- **文件**：`.agents/skills/break-loop/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-21
- **原因**：深度缺陷分析不再作为独立工作流技能保留

#### update-spec

- **文件**：`.agents/skills/update-spec/SKILL.md`
- **改动**：从项目工作流中移除
- **日期**：2026-03-21
- **原因**：Spec 维护改在日常工作中内联完成

### 脚本变更

#### .trellis/scripts

- **改动**：从项目工作流中完全移除
- **日期**：2026-03-20
- **原因**：基于脚本的编排过于复杂且脆弱

### 文档变更

#### 工作流引用

- **文件**：
  - `.trellis/workflow.md`
  - `.trellis/tasks/00-bootstrap-guidelines/prd.md`
- **改动**：将引用从旧的分拆工作流更新为新的合并工作流
- **日期**：2026-03-20

#### 补丁手册

- **文件**：`dev/trellis-local/PATCH-PLAYBOOK.md`
- **目的**：记录确切的本地 Trellis 补丁步骤，便于日后重放
- **添加日期**：2026-03-20
- **原因**：让以后的 AI 会话能单从一份文档重应用定制

#### 技能资源模板

- **文件**：
  - `.agents/skills/start/assets/prd-template.md`
  - `.agents/skills/start/assets/notes-template.md`
  - `.agents/skills/writing-plans/assets/plan-template.md`
- **改动**：在对应技能下增加文件模板
- **日期**：2026-03-20
- **原因**：符合技能目录规则，模板靠近工作流入口

---

## 变更日志

### 2026-03-20

- 新增 `before-dev`
- 新增 `check-dev`
- 为 `prd.md`、`notes.md`、`journal.md` 新增技能资源模板
- 移除 `before-frontend-dev`
- 移除 `before-backend-dev`
- 移除 `check-frontend`
- 移除 `check-backend`
- 移除 `check-cross-layer`
- 移除 `onboard`
- 移除 `create-command`
- 移除 `integrate-skill`
- 移除 `.trellis/scripts/`

### 2026-03-21

- 将 `writing-plans` 本地化为任务目录模型
- 新增可选 `executing-plans`
- 新增可选 `subagent-driven-development`
- 允许从简单 `prd.md` 以及 `plan.md` 执行
- 明确 `before-dev` 在改代码前执行、`check-dev` 在改代码后执行
- 移除 `break-loop`
- 移除 `update-spec`

### 2026-03-22

- 移除 `record-session`
- 停止在活跃工作流中维护 `journal.md`
- 扩充 `notes.md` 以承载交接、验证与提交信息
