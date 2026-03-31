# 任务目录命名补齐日期前缀 实施计划

> 编写要求：每个步骤都要写清涉及文件、完成标志、验证方式。不要写 `TODO`、`待补充`、`按现有模式处理`、`补测试`、`类似上一步` 这类占位话术。

## 目标

把 local-workflow 中与新建任务目录相关的文档统一改成 `MM-DD-brief-topic` 单层命名规则。

## 范围

- 更新 workflow 与直接参与建任务的 skill 文档
- 同步内部 patch playbook，减少后续文档漂移
- 不实现自动生成器，不调整任务目录层级

## 涉及文件

- `.trellis/workflow.md`：补充任务目录命名建议与原因
- `skills/start/SKILL.md`：约束新任务目录命名格式
- `skills/brainstorm/SKILL.md`：约束头脑风暴阶段创建任务时的命名格式
- `dev/trellis-local/PATCH-PLAYBOOK.md`：同步记录该约定

## 执行步骤

1. 创建任务文档并固定实施边界
   - 涉及文件：`.trellis/tasks/03-31-standardize-task-date-prefix/prd.md`、`.trellis/tasks/03-31-standardize-task-date-prefix/notes.md`、`.trellis/tasks/03-31-standardize-task-date-prefix/plan.md`
   - 完成标志：任务目标、范围、涉及文件与执行步骤都已写清
   - 验证方式：人工核对任务文档是否与当前需求一致

2. 回写工作流与建任务入口文档
   - 涉及文件：`.trellis/workflow.md`、`skills/start/SKILL.md`、`skills/brainstorm/SKILL.md`
   - 完成标志：三个文件都明确写出 `MM-DD-brief-topic` 单层目录格式，并说明不使用多级目录的原因
   - 验证方式：使用文本比对确认三个文件命名规则一致

3. 同步内部维护文档并做一致性收口
   - 涉及文件：`dev/trellis-local/PATCH-PLAYBOOK.md`
   - 完成标志：playbook 里也记录了同一命名规范
   - 验证方式：人工检查维护文档与外部工作流文档口径一致

## 验证点

- [ ] 新任务目录命名建议已在 workflow 中明确出现
- [ ] start / brainstorm 两个入口文件对齐同一规则
- [ ] patch playbook 已同步相同规则

## 风险与备注

- 本次只补文档，不会自动改写旧任务目录名
- 若未来要引入 task 生成器，应直接生成单层 `MM-DD-brief-topic` 目录，避免再引入多级结构
