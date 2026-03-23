# local-workflow

面向 Codex 的本地 Trellis 工作流基线。

## 结构

- `skills/`：公开分发的全局技能源；安装时通过 symlink 挂到 `~/.agents/skills/local-workflow`
- `.trellis/`：唯一维护、唯一分发的项目工作流基线
- `dev/trellis-local/`：本仓库私有的 Trellis 定制说明、补丁手册与同步排除清单
- `INSTALL.md`：唯一安装 / 更新入口

## 设计约束

- `skills/` 是仓库内的真实技能源，不再通过仓库内 symlink 包装
- `.trellis/` 直接通过 `rsync` 合并到目标项目，并使用 `dev/trellis-local/rsync-excludes.txt` 排除开发者本地状态与临时文件
- 不再维护 `project/.trellis/` 这类发布副本
- 本地私有说明、补丁手册和分发辅助文件统一放在 `dev/` 目录，不进入目标项目
- `.trellis/.developer` 属于开发者本地状态，不进入目标项目

## 给 AI 的单入口

直接发送：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/beilo/local-workflow/refs/heads/master/INSTALL.md
```

该入口会先检测当前项目状态，再自动决定走 install 还是 update：

- 2 个 marker 都存在：update
- 2 个 marker 都不存在：install
- marker 只存在一部分：停止并报告状态异常

检测 marker：

- `.trellis/.version`
- `.trellis/workflow.md`

## 首次接入后

安装完成后，优先处理：

- `.trellis/tasks/00-bootstrap-guidelines/`

这个任务用于把 `spec/` 从模板内容补齐为项目真实约定。若跳过这一步，后续 AI 更容易生成偏通用的代码与文档。
