---
name: before-dev
description: "在开始任务前阅读相关的开发指南。"
---

# 开发前准备

在开始开发任务之前，先阅读相关的开发指南。

执行以下步骤：

## 步骤 1：检测包管理器

查看项目根目录的 lockfile，确定当前项目使用的包管理器：

| 文件 | 包管理器 |
|------|----------|
| `bun.lockb` 或 `bun.lock` | `bun` |
| `pnpm-lock.yaml` | `pnpm` |
| `yarn.lock` | `yarn` |
| `package-lock.json` | `npm` |
| 均不存在 | 查看 `package.json` 的 `packageManager` 字段，或默认用 `npm` |

将检测结果写入当前任务的 `notes.md`，格式如下：

```
## 环境

- 包管理器：<检测结果，如 pnpm>
```

后续所有需要执行包管理器命令的步骤，均使用此处记录的值。

## 步骤 2：明确任务范围并阅读指南

1. 先明确任务范围：
   - 前端任务 → 阅读前端指南
   - 后端任务 → 阅读后端指南
   - 全栈或范围不清 → 两边都读
2. 阅读对应的索引文件：
   - `.trellis/spec/frontend/index.md`
   - `.trellis/spec/backend/index.md`
3. 根据任务阅读详细指南文件：
   - 前端组件工作 → `.trellis/spec/frontend/component-guidelines.md`
   - 前端 Hook 工作 → `.trellis/spec/frontend/hook-guidelines.md`
   - 前端状态工作 → `.trellis/spec/frontend/state-management.md`
   - 前端类型工作 → `.trellis/spec/frontend/type-safety.md`
   - 后端数据库工作 → `.trellis/spec/backend/database-guidelines.md`
   - 后端错误处理 → `.trellis/spec/backend/error-handling.md`
   - 后端日志 → `.trellis/spec/backend/logging-guidelines.md`
4. 若任务跨多层，另读：
   - `.trellis/spec/guides/cross-layer-thinking-guide.md`
   - `.trellis/spec/guides/code-reuse-thinking-guide.md`
5. 理解需要遵循的编码标准与模式
6. 再推进开发计划

## 注意：spec 空模板降级

若读取 spec 文件时，内容仍为占位符（含"待填写"字样、或主体为空），
**跳过该文件，不要把空模板当作有效约定**，并提示：

> `.trellis/spec/` 尚未初始化，建议先完成 `00-bootstrap-guidelines` 任务，
> 再开始本次开发，否则 AI 生成的代码将缺乏项目上下文。

若用户明确要求继续，则按通用最佳实践推进，但须在 `notes.md` 中注明"spec 未初始化，本次按通用惯例执行"。

---

**写任何代码之前**，本步骤为必做项。
