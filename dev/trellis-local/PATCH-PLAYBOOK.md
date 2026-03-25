# Trellis 本地定制补丁手册

这份手册描述当前仓库的**目标状态**。

目标不是继续修补旧脚本系统。
目标是把 local-workflow 收敛为：

- 仓库内使用 `skills/` 作为真实技能源
- 安装时分发到目标项目根目录 `skills/`
- 只维护一份根目录 `.trellis/`
- 通过文件驱动任务管理
- 不再使用 `.trellis/scripts/`

---

## 当前目标状态

### 仓库结构

保留：

- `skills/`
- `.trellis/`
- `dev/trellis-local/`

移除：

- `project/.trellis/`
- `.trellis/.template-hashes.json`
- 旧的 `.trellis/scripts/`

### skills 分层

默认工作流技能：

- `skills/start/`
- `skills/brainstorm/`
- `skills/before-dev/`
- `skills/check-dev/`
- `skills/finish-work/`

可选执行技能：

- `skills/writing-plans/`
- `skills/executing-plans/`
- `skills/subagent-driven-development/`

已移除：

- `skills/onboard/`
- `skills/create-command/`
- `skills/integrate-skill/`
- `skills/break-loop/`
- `skills/update-spec/`
- 旧的 split skills
- `record-session`

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

- `notes.md` 同时记录任务状态、验证结果与交接信息
- 不再单独维护 `journal.md`

### 分发规则

- 根目录 `skills/` 与 `.trellis/` 是分发源
- 安装与更新统一从 `~/local-workflow/skills/` 同步到目标项目 `skills/`
- 安装与更新统一从 `~/local-workflow/.trellis/` 同步到目标项目 `.trellis/`
- 同步 `.trellis/` 时统一使用 `~/local-workflow/dev/trellis-local/rsync-excludes.txt`
- `dev/` 内的私有说明与辅助文件不得同步进目标项目

---

## 修改清单

### 一、删除旧脚本系统

删除整个目录：

- `.trellis/scripts/`

如果这些文件仍然存在，也一并删除：

- `.trellis/config.yaml`
- `.trellis/worktree.yaml`

### 二、收敛 skills 目录

保留默认工作流技能：

- `start`
- `brainstorm`
- `before-dev`
- `check-dev`
- `finish-work`

保留可选执行技能：

- `writing-plans`
- `executing-plans`
- `subagent-driven-development`

删除：

- `onboard`
- `create-command`
- `integrate-skill`
- `before-frontend-dev`
- `before-backend-dev`
- `check-frontend`
- `check-backend`
- `check-cross-layer`
- `record-session`

### 三、在 skills 内放模板

按 skills 目录规则，把模板放进对应 skill 的 `assets/`：

#### `skills/start/assets/`

- `prd-template.md`
- `notes-template.md`

用途：

- 新建任务时直接参考这两个模板

#### `skills/writing-plans/assets/`

- `plan-template.md`

用途：

- 在当前任务目录中生成 `plan.md`

### 四、更新工作流文档

#### `skills/start/SKILL.md`

改成纯文件驱动流程：

- 读 `workflow.md`
- 读 spec
- 读任务目录
- 若 `spec/` 仍是模板内容，优先处理 `00-bootstrap-guidelines`
- 新任务手动创建 `prd.md` 和 `notes.md`
- 不再调用任何 `.trellis/scripts/*`

#### `.trellis/workflow.md`

改成：

- 不再出现旧脚本编排
- 任务改成 `prd.md + plan.md + notes.md`
- `notes.md` 承担状态、验证与交接
- 若 `spec/` 仍是模板内容，优先处理 `00-bootstrap-guidelines`

#### 可选执行技能

保留这些技能，但不放进默认流程：

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

### 五、收敛分发结构

保留：

- 根目录 `.trellis/`

移除：

- `project/.trellis/`
- `.trellis/.template-hashes.json`

安装与更新统一改为：

- 从 `~/local-workflow/skills/` 同步到目标项目 `skills/`
- 从 `~/local-workflow/.trellis/` 同步到目标项目 `.trellis/`
- 同步 `.trellis/` 时使用 `~/local-workflow/dev/trellis-local/rsync-excludes.txt` 排除临时文件

### 六、实际文件状态

需要确保这些文件真实存在：

- `skills/start/assets/prd-template.md`
- `skills/start/assets/notes-template.md`
- `skills/writing-plans/assets/plan-template.md`
- `skills/writing-plans/SKILL.md`
- `skills/executing-plans/SKILL.md`
- `skills/subagent-driven-development/SKILL.md`
- `dev/trellis-local/rsync-excludes.txt`

---

## 验证步骤

只做轻量验证。

### 验证 1

确认项目里不再有：

- `.trellis/scripts/`
- `project/.trellis/`
- `.trellis/.template-hashes.json`
- `record-session`

### 验证 2

确认技能模板存在：

- `skills/start/assets/prd-template.md`
- `skills/start/assets/notes-template.md`
- `skills/writing-plans/assets/plan-template.md`

### 验证 3

确认会话与任务分离：

- task 用 `prd.md`、`notes.md`
- `notes.md` 承担状态、验证、交接与提交信息

### 验证 4

确认分发规则一致：

- `INSTALL.md` 同时同步 `skills/` 与 `.trellis/`
- `.trellis/` 同步命令使用 `rsync-excludes.txt`
- 不再依赖全局 `~/.agents/skills/local-workflow`
- `dev/` 内容不进入目标项目

---

## 给 AI 的执行说明

以后如果要恢复或重放这次改造，直接把下面这段话发给 AI：

> 读取 `dev/trellis-local/PATCH-PLAYBOOK.md`，把 local-workflow 收敛为 `skills/ + .trellis/ + dev/` 三段结构。仓库内使用 `skills/` 作为真实技能源，安装时同步到目标项目根目录 `skills/`。删除 `.trellis/scripts/`、`project/.trellis/` 和 `.trellis/.template-hashes.json`。任务目录使用 `prd.md`、按需使用 `plan.md`、并维护 `notes.md`。`notes.md` 同时承担状态、验证和交接。`executing-plans` 和 `subagent-driven-development` 作为可选执行技能，接受 `prd.md`、`plan.md` 或用户提供的任务文档。真正开始改代码前先做 `before-dev`，代码改完后做 `check-dev`。安装与更新统一从根目录 `skills/` 与 `.trellis/` 同步；同步 `.trellis/` 时使用 `dev/trellis-local/rsync-excludes.txt` 排除临时文件。修改文件必须使用 `apply_patch`。不要生成测试脚本，不要执行编译。完成后只做文档中列出的轻量验证。
