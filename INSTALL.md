# local-workflow install/update instructions

This is the single entrypoint for both humans and AI.
It handles detection first, then chooses install or update.

It is meant to be used in the same style as:

`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md`

---

## One prompt to copy for AI

```text
Fetch and follow instructions from ~/local-workflow/INSTALL.md.
First detect whether the current project already has local-workflow installed.
Use these markers in the project root for detection: .trellis/.version, .trellis/.template-hashes.json, and .trellis/workflow.md.
If all markers exist, run the update flow.
If all markers are missing, run the install flow.
If only some markers exist, stop and report an inconsistent local-workflow state instead of guessing.
When running update, always run a dry-run first with rsync -ani and report which files would change before running the real sync.
Never delete the target project's .trellis directory as part of a normal update.
Never use rsync --delete for a normal update.
Use rsync --delete only when explicitly asked to force the target project to mirror ~/local-workflow/project/.trellis exactly.
```

---

## Layout

This repo is used in three parts:

- `~/local-workflow/skills/` — global skill source for Codex
- `~/local-workflow/project/.trellis/` — files to merge into a target project root

---

## Detection rules

Treat the project as already installed only when all of these markers exist in the project root:

- `.trellis/.version`
- `.trellis/.template-hashes.json`
- `.trellis/workflow.md`

Decision table:

- all markers exist -> update
- all markers missing -> install
- partial markers exist -> stop and report an inconsistent local-workflow state instead of guessing

---

## First-time install flow

Use this only when all three markers are missing.

1. Clone this repository:

   ```bash
   git clone <your-local-workflow-repo-url> ~/local-workflow
   ```

2. Create the global skills symlink once:

   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/local-workflow/skills ~/.agents/skills/local-workflow
   ```

3. Merge `.trellis/` into the target project root:

   ```bash
   mkdir -p /path/to/target-project/.trellis
   rsync -a ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
   ```

4. Restart Codex so the new skills are discovered.

This flow is merge-based.
It does not require deleting the existing project `.trellis/` first.

---

## Update flow

Use this only when all three markers already exist.

1. Update the local-workflow repo:

   ```bash
   cd ~/local-workflow && git pull
   ```

2. Keep the existing skills symlink as-is.

   If `~/.agents/skills/local-workflow` already points to `~/local-workflow/skills`, do nothing.

3. Run a dry-run first:

   ```bash
   rsync -ani ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
   ```

4. Merge the latest `.trellis/` files into the target project:

   ```bash
   rsync -a ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
   ```

This updates shipped files in place and keeps project-local files that are not part of local-workflow.

---

## When to use `--delete`

Default updates should not use `--delete`.

Only use `--delete` when you intentionally want the target project's `.trellis/` to become an exact mirror of `~/local-workflow/project/.trellis/` and you have confirmed there is no project-specific content to keep.

```bash
rsync -a --delete ~/local-workflow/project/.trellis/ /path/to/target-project/.trellis/
```

---

## Hard guardrails

- Never run install on a project that already contains all local-workflow markers.
- Never run update on a project that contains none of the local-workflow markers.
- If only some markers exist, stop and ask for manual cleanup or confirmation.
- Never delete the target project's `.trellis` directory as part of a normal update.
- Never use `rsync --delete` for a normal update.
- Any local-only skill or patch playbook must stay outside this repository, for example in an external DdV folder that is not tracked by git.
- `.trellis/.developer` is developer-local state and should not be shipped into target projects.

---

## Verify

```bash
ls -ld ~/.agents/skills/local-workflow
find /path/to/target-project/.trellis -maxdepth 2 -type f | sort
```

You should see:

- a symlink under `~/.agents/skills/` pointing to `~/local-workflow/skills`
- a `.trellis/` directory inside the target project root
- no local-only private content copied into the project `.trellis/`

---

## Uninstalling

```bash
rm ~/.agents/skills/local-workflow
rm -rf /path/to/target-project/.trellis
```

Optionally remove the clone:

```bash
rm -rf ~/local-workflow
```
