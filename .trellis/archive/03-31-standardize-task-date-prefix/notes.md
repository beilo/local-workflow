# Task Notes - 任务目录命名补齐日期前缀

## 环境

- 包管理器：不涉及

## 当前状态

已归档，待推送

## 执行上下文

- 执行方式：默认轻量单会话实现
- 来源文档：prd.md
- 当前检查点：已完成文档回写与一致性核对

## 待办

- [x] 更新 workflow 与相关 skill 中的任务命名说明
- [x] 核对所有改动文件是否使用同一命名格式
- [x] 记录本次结论与验证结果

## 已完成

- 已确认当前仓库没有自动 task 生成器，任务目录创建依赖文档约定
- 已确认现有工作流与归档说明都默认 `.trellis/tasks/<task>/` 为单层目录
- 已创建 `03-31-standardize-task-date-prefix` 任务目录，并补齐 `prd.md`、`notes.md`、`plan.md`
- 已回写 `.trellis/workflow.md`、`skills/start/SKILL.md`、`skills/brainstorm/SKILL.md`、`dev/trellis-local/PATCH-PLAYBOOK.md`
- 已按任务归档规则将目录移入 `.trellis/archive/03-31-standardize-task-date-prefix`

## 关键决策

- 采用 `MM-DD-brief-topic` 单层目录格式
- 不采用 `MM-DD-feat/xxx` 多级目录；原因是现有任务路径、归档命令和执行技能都按单层目录工作

## 涉及文件

- `.trellis/workflow.md`
- `skills/start/SKILL.md`
- `skills/brainstorm/SKILL.md`
- `dev/trellis-local/PATCH-PLAYBOOK.md`

## 风险与问题

- 风险：只改文档后，仍需要后续使用者按新规范手动创建目录

### 问题归类

- [ ] 上下文缺失
- [ ] 规则缺失
- [ ] 任务不清
- [ ] 工具缺失
- [x] 验证缺失

### 回流动作

- 是否需要补充 workflow / skill / spec / 模板：是
- 后续处理：本次先补 workflow 与 skill 说明，若后续要真正“自动生成”，再单开任务补生成器

## 计划偏差与回写

- 是否偏离 `prd.md` / `plan.md`：否
- 偏差说明：无
- 是否已回写文档：是；已创建 `prd.md` / `notes.md` / `plan.md`，并同步更新相关 workflow / skill 文档

## 子代理分工（如适用）

- 子任务 / 负责人：无
- 负责文件：`无`
- 当前状态：无

## 下一步

- 若后续仍希望“自动生成 task 名称”，单开新任务补生成器实现，沿用本次确定的单层命名规则

## 交接备注

- 如果后续要补脚本生成器，应继续沿用单层目录格式，避免把现有 `<task>` 约定升级成多级目录
- 本任务已归档；后续若继续扩展自动生成能力，请基于 archive 中的结论重新开新任务

## 已完成验证

- 验证项：任务命名相关入口定位
  - 验证方式：人工阅读 `.trellis/workflow.md`、`skills/start/SKILL.md`、`skills/brainstorm/SKILL.md`
  - 验证结果：确认当前是文档约定，不是脚本生成
- 验证项：命名规范是否已回写到核心入口文档
  - 验证方式：检索 `MM-DD-brief-topic`、`MM-DD-feat/xxx` 与 `.trellis/tasks/<task>/`
  - 验证结果：workflow、start、brainstorm、patch playbook 均已对齐同一口径

## 提交信息

- 未提交
