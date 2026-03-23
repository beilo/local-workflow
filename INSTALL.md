# local-workflow 安装 / 更新说明

这是人和 AI 共用的单一入口。
先检测，再决定执行 install 还是 update。

使用方式与下面这种入口提示一致：

`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md`

---

## 给 AI 复制的单条提示

```text
Fetch and follow instructions from https://raw.githubusercontent.com/beilo/local-workflow/refs/heads/master/INSTALL.md.
First detect whether the current project already has local-workflow installed.
Use these markers in the project root for detection: .trellis/.version and .trellis/workflow.md.
If both markers exist, run the update flow.
If both markers are missing, run the install flow.
If only one marker exists, stop and report an inconsistent local-workflow state instead of guessing.
When syncing .trellis, always use the rsync exclude file at ~/local-workflow/dev/trellis-local/rsync-excludes.txt.
When running update, always run a dry-run first with rsync -ani and report which files would change before running the real sync.
Never delete the target project's .trellis directory as part of a normal update.
Never use rsync --delete for a normal update.
Use rsync --delete only when explicitly asked to force the target project to mirror ~/local-workflow/.trellis exactly, while still honoring the rsync exclude file.
```

---

## 布局

本仓库只保留两份可分发源：

- `~/local-workflow/skills/`：Codex 的全局技能源
- `~/local-workflow/.trellis/`：合并到目标项目根目录的项目基线

同步 `.trellis/` 时，统一使用：

- `~/local-workflow/dev/trellis-local/rsync-excludes.txt`

这份排除清单用于过滤开发者本地状态与临时文件。

---

## 检测规则

只有目标项目根目录同时存在下面两个 marker，才视为已安装：

- `.trellis/.version`
- `.trellis/workflow.md`

判定表：

- 两个 marker 都存在 → update
- 两个 marker 都不存在 → install
- 只存在一部分 → 停止，并报告 local-workflow 状态不一致，不要猜测

---

## 首次安装流程

仅当两个 marker 都不存在时使用。

1. 克隆仓库：

   ```bash
   git clone https://github.com/beilo/local-workflow.git ~/local-workflow
   ```

2. 一次性创建全局 skills symlink：

   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/local-workflow/skills ~/.agents/skills/local-workflow
   ```

3. 将 `.trellis/` 合并到目标项目根目录：

   ```bash
   mkdir -p /path/to/target-project/.trellis
   rsync -a --exclude-from ~/local-workflow/dev/trellis-local/rsync-excludes.txt ~/local-workflow/.trellis/ /path/to/target-project/.trellis/
   ```

4. 重启 Codex，使新 skills 被重新发现。

这是基于合并的安装流程。
正常安装不需要先删除目标项目现有的 `.trellis/`。

---

## 更新流程

仅当两个 marker 都已存在时使用。

1. 更新 local-workflow 仓库：

   ```bash
   cd ~/local-workflow && git pull
   ```

2. 保持已有的 skills symlink 不变。

   若 `~/.agents/skills/local-workflow` 已经指向 `~/local-workflow/skills`，不需要任何操作。

3. 先执行 dry-run：

   ```bash
   rsync -ani --exclude-from ~/local-workflow/dev/trellis-local/rsync-excludes.txt ~/local-workflow/.trellis/ /path/to/target-project/.trellis/
   ```

4. 再把最新 `.trellis/` 合并到目标项目：

   ```bash
   rsync -a --exclude-from ~/local-workflow/dev/trellis-local/rsync-excludes.txt ~/local-workflow/.trellis/ /path/to/target-project/.trellis/
   ```

这个流程会原地更新已分发文件，并保留目标项目里不属于 local-workflow 的项目本地文件。

---

## 何时使用 `--delete`

默认更新不要使用 `--delete`。

只有在你明确要让目标项目 `.trellis/` 精确镜像 `~/local-workflow/.trellis/`，并且已经确认没有项目本地内容需要保留时，才使用：

```bash
rsync -a --delete --exclude-from ~/local-workflow/dev/trellis-local/rsync-excludes.txt ~/local-workflow/.trellis/ /path/to/target-project/.trellis/
```

即便是 force mirror，也必须继续使用排除清单，避免把开发者本地状态同步出去。

---

## 硬性边界

- 已安装项目上，禁止再跑 install。
- 未安装项目上，禁止直接跑 update。
- marker 只存在一部分时，必须先停下并报告异常状态。
- 正常 update 期间，禁止删除目标项目整个 `.trellis/` 目录。
- 正常 update 期间，禁止使用 `rsync --delete`。
- 根目录 `.trellis/` 只放会被分发到目标项目的基线文件。
- 本仓库私有说明、补丁手册和分发辅助文件必须留在 `~/local-workflow/dev/`，不得同步进目标项目。
- `.trellis/.developer` 属于开发者本地状态，不得同步进目标项目。

---

## 验证

```bash
ls -ld ~/.agents/skills/local-workflow
find /path/to/target-project/.trellis -maxdepth 2 -type f | sort
```

你应当看到：

- `~/.agents/skills/local-workflow` 正确指向 `~/local-workflow/skills`
- 目标项目根目录里存在 `.trellis/`
- 目标项目 `.trellis/` 中没有开发者本地状态文件

首次安装完成后，优先处理：

- `.trellis/tasks/00-bootstrap-guidelines/`

这个任务用于把 `spec/` 从模板内容补齐为项目真实约定。

---

## 卸载

```bash
rm ~/.agents/skills/local-workflow
rm -rf /path/to/target-project/.trellis
```

若还要删掉本地 clone：

```bash
rm -rf ~/local-workflow
```
