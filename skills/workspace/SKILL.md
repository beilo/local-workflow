---
name: workspace
description: "交互式创建独立 git worktree 的可选工作流。"
---

# 工作区创建

在你需要**隔离当前改动**、**并行推进另一个任务**，或者**保持主工作区干净**时使用本技能。

当前版本是 **create-only**：

- 只支持创建工作区
- 不支持 `workspace complete`
- 不自动 `fetch` / `pull`
- 不自动 stash、清理或修复脏工作区
- 工作区根目录固定为 `~/worktree/{repo}/{name}`

## 使用原则

1. **先检查仓库状态**
   - 必须先确认当前目录位于 git 仓库中
   - 必须确认当前工作区干净；若有未提交改动，停止并提示用户先处理

2. **先列本地分支，再让用户选择**
   - 不要直接猜测基准分支
   - 使用本地分支列表作为候选，再让用户选择
   - 若用户要的分支不在本地列表里，停止并提示先把该分支带到本地，再重新执行

3. **把“基准分支”和“新工作分支”区分开**
   - 用户选择的是**基准分支**
   - 真正创建出来的是一个**新的工作分支**
   - 新工作分支名与工作区目录名保持一致

> 原因：同一个分支不能同时被多个 worktree 检出。

## 创建流程

### 步骤 1：确认当前目录可创建 worktree

运行：

```bash
git rev-parse --show-toplevel
git status --short
```

要求：

- `git rev-parse` 成功，能拿到仓库根目录
- `git status --short` 为空；若不为空，停止并报告当前未提交改动

### 步骤 2：收集仓库名与候选分支

仓库名取当前仓库根目录名：

```bash
basename "$(git rev-parse --show-toplevel)"
```

候选分支只列**本地分支**：

```bash
git for-each-ref --sort=-committerdate refs/heads --format='%(refname:short)'
```

展示建议：

- 优先展示最近更新的分支
- 数量控制在 10 个以内
- 若 `main` / `master` 在本地存在，尽量包含在展示列表中

然后只问用户一个问题：

```markdown
我准备创建新的 workspace。下面是当前本地分支候选：

1. `<branch-a>`
2. `<branch-b>`
3. `<branch-c>`

你要基于哪个分支创建？
```

### 步骤 3：确定新工作区名称

命名规则：

- 若用户已经在请求里给出名称，优先使用该名称
- 若用户未给名称，自动生成：`ws-YYYYMMDD-HHMMSS`
- 将名称中的 `/` 与空格替换为 `-`

同一个名称同时用于：

- 新工作区目录名
- 新工作分支名

若目标目录已存在，或新分支名已存在，则在名称后追加时间戳后重试，不要覆盖已有工作区。

### 步骤 4：创建目标目录与新 worktree

目标路径固定为：

```text
~/worktree/{repo}/{name}
```

创建命令骨架：

```bash
mkdir -p ~/worktree/"$repo"
git worktree add -b "$name" ~/worktree/"$repo"/"$name" "$base_branch"
```

其中：

- `$name`：上一步得到的新工作区名称
- `$base_branch`：用户选择的基准分支

### 步骤 5：报告结果与下一步

创建成功后，明确输出：

- 新工作区路径
- 基准分支
- 新工作分支名
- 下一步动作

建议输出格式：

```markdown
已创建 workspace：

- 路径：`~/worktree/<repo>/<name>`
- 基准分支：`<base-branch>`
- 新工作分支：`<name>`

下一步：

1. `cd ~/worktree/<repo>/<name>`
2. 在新工作区执行 `$start`
```

## 边界

当前不要做这些事：

- 不实现 `complete`
- 不写外部 metadata
- 不自动合并或创建 PR
- 不自动更新基准分支
- 不在脏工作区上强行创建 worktree

