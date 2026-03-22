# Trellis 本地定制补丁手册

这份手册描述当前项目的**最终目标状态**。

目标不是继续修补旧脚本系统。
目标是把工作流收敛为：

- 纯 skills 入口
- 文件驱动任务管理
- 不再使用 `.trellis/scripts/`

---

## 当前目标状态

### skills 入口

保留：

- `.agents/skills/start/`
- `.agents/skills/brainstorm/`
- `.agents/skills/before-dev/`
- `.agents/skills/check-dev/`
- `.agents/skills/finish-work/`
- `.agents/skills/writing-plans/`
- `.agents/skills/trellis-local/`

删除：

- `.agents/skills/onboard/`
- `.agents/skills/create-command/`
- `.agents/skills/integrate-skill/`
- `.agents/skills/break-loop/`
- `.agents/skills/update-spec/`
- 旧的 split skills

### 任务文件

每个任务目录至少保留：

- `prd.md`
- `notes.md`

按需增加：

- `plan.md`

不再依赖：

- `task.json`
- `implement.jsonl`
- `check.jsonl`
- `debug.jsonl`

### 任务记录

规则：

- `notes.md` 同时记录任务状态与交接信息
- 不再单独维护 `journal.md`

---

## 修改清单

### 一、删除脚本系统

删除整个目录：

- `.trellis/scripts/`

如果这些文件仍然存在，也一并删除：

- `.trellis/config.yaml`
- `.trellis/worktree.yaml`

### 二、保留并收敛 skills

保留并使用：

- `start`
- `before-dev`
- `check-dev`
- `finish-work`
- `trellis-local`

删除：

- `onboard`
- `create-command`
- `integrate-skill`
- `before-frontend-dev`
- `before-backend-dev`
- `check-frontend`
- `check-backend`
- `check-cross-layer`

### 三、在 skills 内放模板

按 skills 目录规则，把模板放进对应 skill 的 `assets/`：

#### `start/assets/`

- `prd-template.md`
- `notes-template.md`

用途：

- 新建任务时直接参考这两个模板

#### `writing-plans/assets/`

- `plan-template.md`

用途：

- 在当前任务目录中生成 `plan.md`

### 四、更新技能文档

#### `start/SKILL.md`

改成纯文件驱动流程：

- 读 `workflow.md`
- 读 spec
- 读 `.trellis/.developer`
- 读任务目录
- 新任务手动创建 `prd.md` 和 `notes.md`
- 不再调用任何 `.trellis/scripts/*`

#### `record-session/SKILL.md`

删除。

#### `workflow.md`

改成：

- 不再出现 script、task.py、jsonl、worktree、registry、多 agent 流程
- 任务改成 `prd.md + plan.md + notes.md`
- 不再使用单独的会话日志

#### 新增可选技能

新增这些技能，但不放进默认流程：

- `writing-plans`
- `executing-plans`
- `subagent-driven-development`

规则：

- `writing-plans` 本地化写入 `.trellis/tasks/<task>/plan.md`
- `executing-plans` 和 `subagent-driven-development` 都接受：
  - `prd.md`
  - `plan.md`
  - 用户显式提供的任务文档
- 简单 `prd.md` 可以直接执行
- 复杂 `prd.md` 先转成 `plan.md`
- 真正开始改代码前，先做 `before-dev`
- 代码改完后，做 `check-dev`

### 五、实际文件状态

需要确保这些文件真实存在：

- `.agents/skills/start/assets/prd-template.md`
- `.agents/skills/start/assets/notes-template.md`
- `.agents/skills/record-session/assets/journal-template.md`
- `.agents/skills/writing-plans/assets/plan-template.md`
- `.agents/skills/writing-plans/SKILL.md`
- `.agents/skills/executing-plans/SKILL.md`
- `.agents/skills/subagent-driven-development/SKILL.md`

---

## 验证步骤

只做轻量验证。

### 验证 1

确认项目里不再有：

- `.trellis/scripts/`
- `task.py`
- `add_session.py`
- `get_context.py`
- `get_developer.py`
- `init_developer.py`

### 验证 2

确认技能模板存在：

- `start/assets/prd-template.md`
- `start/assets/notes-template.md`
- `writing-plans/assets/plan-template.md`

### 验证 3

确认会话与任务分离：

- task 用 `prd.md` 和 `notes.md`
- session 用 `journal.md`

### 验证 4

确认 `.trellis/.template-hashes.json`：

- 能正常解析
- 不再包含 `.trellis/scripts/` 文件
- 包含新的 skill assets 和 `journal.md` 相关变更条目

---

## 给 AI 的执行说明

以后如果要恢复或重放这次改造，直接把下面这段话发给 AI：

> 读取 `.agents/skills/trellis-local/PATCH-PLAYBOOK.md`，把 Trellis 改造成纯 skills + 文件驱动工作流。删除 `.trellis/scripts/`，保留 `start / brainstorm / writing-plans / before-dev / check-dev / finish-work / trellis-local`。任务目录使用 `prd.md`、按需使用 `plan.md`、并维护 `notes.md`。`notes.md` 同时承担状态、验证和交接。`executing-plans` 和 `subagent-driven-development` 作为可选执行技能，接受 `prd.md`、`plan.md` 或用户提供的任务文档。真正开始改代码前先做 `before-dev`，代码改完后做 `check-dev`。模板放在对应 skill 的 `assets/` 下。修改文件必须使用 `apply_patch`。不要生成测试脚本，不要执行编译。完成后只做文档中列出的轻量验证。
