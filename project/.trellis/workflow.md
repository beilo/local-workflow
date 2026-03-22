# 开发工作流

## 核心原则

这套工作流现在是**纯 skills + 文件驱动**。

不再依赖旧的脚本目录。
任务状态和会话记录都直接写入文件。

---

## 核心文件

```text
.trellis/
|-- .developer
|-- workflow.md
|-- spec/
|   |-- frontend/
|   |-- backend/
|   +-- guides/
|-- tasks/
|   +-- <task-name>/
|       |-- prd.md
|       +-- notes.md
```

### 文件职责

#### `prd.md`

记录：

- 目标
- 范围
- 验收标准
- 约束

#### `plan.md`

记录：

- 实施步骤
- 涉及文件
- 验证点
- 风险备注

#### `notes.md`

记录：

- 当前状态
- 待办
- 已完成
- 关键决策
- 涉及文件
- 风险与问题

- 下一步
- 交接备注
- 已完成验证
- 提交信息

---

## 会话开始

1. 阅读 `AGENTS.md`
2. 阅读本文件
3. 阅读：
   - `.trellis/spec/frontend/index.md`
   - `.trellis/spec/backend/index.md`
   - `.trellis/spec/guides/index.md`
4. 运行：
   - `git status`
   - `git log --oneline -10`
5. 阅读 `.trellis/.developer`
6. 在 `.trellis/tasks/` 下找到当前活跃的任务目录

---

## 创建任务

需要新任务时，请手动创建任务目录。

必需文件：

- `prd.md`
- `notes.md`

可选文件：

- `plan.md`

参考模板：

- `.agents/skills/start/assets/prd-template.md`
- `.agents/skills/start/assets/notes-template.md`
- `.agents/skills/writing-plans/assets/plan-template.md`

---

## 开发流程

### 默认轻量流程

1. `$start`
2. `$before-dev`
3. 实现改动
4. 更新 `notes.md`
5. `$check-dev`
6. `$finish-work`
7. 人工测试并提交
8. 更新 `notes.md` 中的最终状态与交接信息

### 需求较重流程

1. `$start`
2. `$brainstorm` -> 将需求落到 `prd.md`
3. 如需，使用 `$writing-plans` 编写 `plan.md`
4. 选择执行方式：
   - 默认单会话实现
   - 可选 `$executing-plans`
   - 可选 `$subagent-driven-development`

## 执行类文档规则

执行类技能可接受：

- `plan.md`
- `prd.md`
- 用户明确提供的其他任务文档

规则：

- 若文档已可直接执行，则从中直接执行
- 若是简单的 `prd.md`，可推导最小检查清单并直接执行
- 若是复杂的 `prd.md`，先编写 `plan.md`
- 开始改代码前，运行 `before-dev`
- 代码改动结束后，运行 `check-dev`

---

## 最佳实践

1. 写代码前先读 spec
2. 一个任务目录聚焦一个目标
3. 保持 `notes.md` 简短且为最新
4. 用 `notes.md` 承担状态与交接
5. 优先用简单 shell 查看，而非自定义编排
