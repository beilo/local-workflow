# 开发工作流

## 核心原则

这套工作流现在是**纯 skills + 文件驱动**。

不再依赖旧的脚本目录。
任务状态、验证结果和交接信息统一写入任务文件。

---

## 核心文件

```text
.trellis/
|-- workflow.md
|-- spec/
|   |-- frontend/
|   |-- backend/
|   +-- guides/
|-- tasks/
|   +-- <task-name>/          # 活跃任务
|       |-- prd.md
|       |-- notes.md
|       +-- plan.md
+-- archive/                  # 已归档任务（只读参考）
    +-- <task-name>/
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
- 停止条件
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
5. 在 `.trellis/tasks/` 下找到当前活跃的任务目录
6. 若 `.trellis/spec/` 仍主要是模板内容，且 `00-bootstrap-guidelines` 未完成，优先处理该任务

---

## 创建任务

需要新任务时，请手动创建任务目录。

必需文件：

- `prd.md`
- `notes.md`

可选文件：

- `plan.md`

参考模板（安装路径）：

- `skills/start/assets/prd-template.md`
- `skills/start/assets/notes-template.md`
- `skills/writing-plans/assets/plan-template.md`

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
2. `$brainstorm` -> 将需求落到 `prd.md`，完成内联自审并让用户确认
3. 如需，使用 `$writing-plans` 编写 `plan.md`
4. 选择执行方式：
   - 默认轻量单会话实现
   - 可选 `$executing-plans`
   - 可选 `$subagent-driven-development`（仅在任务可清晰拆分时）

## 执行类文档规则

执行类技能可接受：

- `plan.md`
- `prd.md`
- 用户明确提供的其他任务文档

规则：

- 若文档已可直接执行，则从中直接执行
- 若是简单的 `prd.md`，可推导最小检查清单并直接执行
- 若是复杂的 `prd.md`，先编写 `plan.md`
- 若 `plan.md` 已写完，先确认执行方式，再开始改代码
- 开始改代码前，运行 `before-dev`
- 代码改动结束后，运行 `check-dev`

---

## 任务归档

### 归档时机

任务满足以下**任一**条件时，应将其归档：

- 已提交且 `notes.md` 中记录了最终状态与提交信息
- 任务已被放弃，不再推进
- 任务目录在当前会话中不再相关，且距创建已超过合理周期

### 归档操作

```bash
# 将完成的任务移入 .trellis/archive/
mv .trellis/tasks/<task-name> .trellis/archive/<task-name>
```

### 归档规则

- `archive/` 中的任务为**只读参考**，不再修改
- AI 会话开始时**跳过** `archive/`，不纳入上下文扫描
- 若需回溯历史决策，可手动指定 `archive/<task-name>/notes.md` 路径
- `archive/` 目录本身受 git 追踪，保留完整历史

---

## 最佳实践

1. 写代码前先读 spec
2. 首次接入项目时，优先完成 `00-bootstrap-guidelines`
3. 一个任务目录聚焦一个目标
4. 保持 `notes.md` 简短且为最新
5. 用 `notes.md` 承担状态、验证与交接
6. 优先用简单 shell 查看，而非自定义编排
7. **已完成的任务及时归档**，保持 `tasks/` 根目录只有活跃任务
8. `prd.md` / `plan.md` 成稿后先做内联自审，清掉占位符与歧义，再进入执行
