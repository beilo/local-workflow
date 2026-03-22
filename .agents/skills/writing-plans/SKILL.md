---
name: writing-plans
description: "在编码前将实施计划写入当前任务目录。"
---

# 编写计划

在 `prd.md` 已清晰、且尚未开始实现时使用本技能。

## 输出位置

将实施计划写入：

- `.trellis/tasks/<task>/plan.md`

从此模板开始：

- `assets/plan-template.md`

## 输入

阅读：

- 任务的 `prd.md`
- 若存在，任务的 `notes.md`
- `.trellis/spec/` 下相关文件
- 你计划遵循的既有代码模式

## 目标

把当前任务变成简短、具体、可执行的计划。

计划应明确：

- 将修改哪些文件
- 工作顺序
- 每段完成后要验证什么
- 风险点在哪里

## 规则

### 若任务简单

短小的 `plan.md` 即可。

### 若任务为中大型

写更完整的 `plan.md`，包含：

- 阶段
- 涉及文件
- 检查点
- 风险部分的回退说明

## 必备章节

`plan.md` 应包含：

1. 目标
2. 范围
3. 涉及文件
4. 执行步骤
5. 验证点
6. 风险 / 备注

## 与执行的关系

- `brainstorm` 在 `prd.md` 中稳定任务
- `writing-plans` 将其转为 `plan.md`
- `executing-plans` 或 `subagent-driven-development` 随后可使用：
  - `plan.md`
  - 简单的 `prd.md`
  - 用户明确提供的其他任务文档

## 边界

此处不要开始写代码。

本技能只编写或更新 `plan.md`。
