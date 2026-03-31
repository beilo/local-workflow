# 任务目录命名补齐日期前缀

## 目标

把 local-workflow 中“新任务目录如何命名”的约定补清楚，统一收敛为 `MM-DD-brief-topic` 单层格式，避免继续出现不带日期的任务名。

## 范围

- 更新 local-workflow 中与“新建任务目录”直接相关的工作流说明
- 明确为什么选择单层 `MM-DD-brief-topic`，而不是 `MM-DD-feat/xxx`
- 不引入脚本，不新增任务生成器

## 验收标准

- [ ] `.trellis/workflow.md` 明确写出新的任务目录命名建议
- [ ] `skills/start/SKILL.md` 与 `skills/brainstorm/SKILL.md` 对齐同一命名规则
- [ ] `dev/trellis-local/PATCH-PLAYBOOK.md` 同步记录该约定，避免后续文档再漂移

## 约束

- 当前仓库没有自动生成 task 名称的实现，本次只改规范与文档，不虚构脚本能力
- 继续保持 `.trellis/tasks/<task>/` 这套单层目录心智，避免扩大兼容面
