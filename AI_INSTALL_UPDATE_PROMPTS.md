# local-workflow install/update prompt for AI

Use this file as a single entrypoint.
The AI should detect the current project state first, then choose install or update by itself.

It is meant to be invoked in the same style as:

`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md`

---

## One prompt to copy

```text
Fetch and follow instructions from ~/local-workflow/AI_INSTALL_UPDATE_PROMPTS.md.
First detect whether the current project already has local-workflow installed.
Use these markers in the project root for detection: .trellis/.version, .trellis/.template-hashes.json, and .trellis/workflow.md.
If the project already has local-workflow markers, do not run install and run the update flow instead.
If the project does not have local-workflow markers, do not run update and run the install flow instead.
When running update, always run a dry-run first with rsync -ani and report which files would change before running the real sync.
Never delete the target project's .trellis directory as part of a normal update.
Never use rsync --delete for a normal update.
Use rsync --delete only when explicitly asked to force the target project to mirror ~/local-workflow/project/.trellis exactly.
Then execute the correct flow from ~/local-workflow/INSTALL.md.
```

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

## Hard guardrails

```text
Never run install on a project that already contains all local-workflow markers.
Never run update on a project that contains none of the local-workflow markers.
If only some markers exist, stop and ask for manual cleanup or confirmation.
Never delete the target project's .trellis directory as part of a normal update.
Never use rsync --delete for a normal update.
```
