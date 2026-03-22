# local-workflow

面向 Codex 的本地 Trellis 工作流基线。

## 结构

- `skills/`：全局技能源
- `project/.trellis/`：分发到目标项目根目录的 `.trellis` 基线
- `INSTALL.md`：唯一安装/更新入口

## 设计约束

- `skills/` 通过 symlink 挂到 `~/.agents/skills/`
- `project/.trellis/` 通过 `rsync` 合并到目标项目
- `trellis-local` 只保留在本地 skills 源，不进入目标项目
- `.trellis/.developer` 属于开发者本地状态，不进入目标项目

## 给 AI 的单入口

直接发送：

```text
Fetch and follow instructions from ~/local-workflow/INSTALL.md
```

该入口会先检测当前项目状态，再自动决定走 install 还是 update：

- 3 个 marker 都存在：update
- 3 个 marker 都不存在：install
- marker 只存在一部分：停止并报告状态异常

检测 marker：

- `.trellis/.version`
- `.trellis/.template-hashes.json`
- `.trellis/workflow.md`

## 更新基线时

如果你修改了仓库根目录下的 `.trellis/` 源文件，需要重新生成发布副本：

```bash
cd ~/local-workflow
rsync -a --delete --exclude '.developer' .trellis/ project/.trellis/
```
