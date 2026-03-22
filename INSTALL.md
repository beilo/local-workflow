# Installing local-workflow for Codex

Use local-workflow in Codex with a global skills source and a project-local `.trellis/` install.

## Layout

This repo is used in three parts:

- `~/local-workflow/skills/` — global skill source for Codex
- `~/local-workflow/project/.trellis/` — files to merge into a target project root
- `trellis-local` — local-only skill for the developer machine; keep it only in the global skills source and do not copy it into the target project

## Prerequisites

- Git
- Codex CLI with native skill discovery
- `rsync` on macOS/Linux

## First-time setup

1. **Clone this repository:**

   ```bash
   git clone <your-local-workflow-repo-url> ~/local-workflow
   ```

2. **Create the global skills symlink once:**

   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/local-workflow/skills ~/.agents/skills/local-workflow
   ```

3. **Merge `.trellis/` into the target project root:**

   ```bash
   mkdir -p /path/to/target-project/.trellis
   rsync -a ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
   ```

4. **Restart Codex** so the new skills are discovered.

This install flow is merge-based.
It does **not** require deleting the existing project `.trellis/` first.

## Updating an existing installation

When local-workflow changes, update in place.
Do not remove and reinstall everything.

1. **Update the local-workflow repo:**

   ```bash
   cd ~/local-workflow && git pull
   ```

2. **Keep the existing skills symlink as-is.**

   If `~/.agents/skills/local-workflow` already points to `~/local-workflow/skills`, do nothing.
   The symlink will see the new skill files automatically.

3. **Merge the latest `.trellis/` files into the target project:**

   ```bash
   rsync -a ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
   ```

This updates shipped files in place and keeps project-local files that are not part of local-workflow.

## When to use `--delete`

Default updates should **not** use `--delete`.

Only use `--delete` when you intentionally want the target project's `.trellis/` to become an exact mirror of `~/local-workflow/project/.trellis/` and you have confirmed there is no project-specific content to keep.

Example:

```bash
rsync -a --delete ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
```

## Verify

```bash
ls -ld ~/.agents/skills/local-workflow
find /path/to/target-project/.trellis -maxdepth 2 -type f | sort
```

You should see:

- a symlink under `~/.agents/skills/` pointing to `~/local-workflow/skills`
- a `.trellis/` directory inside the target project root
- no `trellis-local` content copied into the project `.trellis/`

## Notes

- `trellis-local` stays in the global skills source only.
- `.trellis/.developer` is developer-local state and should not be shipped into target projects.
- If a target project already customized some `.trellis/` files, review diffs before overwriting them during an update.

## Uninstalling

```bash
rm ~/.agents/skills/local-workflow
rm -rf /path/to/target-project/.trellis
```

Optionally remove the clone:

```bash
rm -rf ~/local-workflow
```
