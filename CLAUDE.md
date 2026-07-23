# Vibe to Spec — root configuration

This file is loaded in every Claude Code session started anywhere in this repository, regardless of which step folder the session is working from. Its only job is finding the active project's records and external code before any step-specific `CLAUDE.md` takes over. It carries no step-specific rules, so nothing about a step's own discipline leaks between steps.

## Finding the active project

`.active_project.vibe_to_spec.yaml`, at the repository root, is a symlink pointing at `projects/<project-name>.vibe_to_spec.yaml` — the config file for whichever project is currently active.

At the start of every session:

1. Resolve `.active_project.vibe_to_spec.yaml`. If the symlink does not exist, or it exists but its target file is missing, treat this as first-ever use: ask the user which project to work on, create `projects/<project-name>.vibe_to_spec.yaml` if it does not exist yet (see "Creating a new project" below), then create the symlink pointing at it.
2. If it resolves, read the project name from the target file's name and show it to the user: "Active project: `<project-name>`. Continue with this project, or switch?" Never proceed without this confirmation, even when the symlink already exists from an earlier session.
3. To switch projects, repoint the symlink at a different `projects/*.vibe_to_spec.yaml` file — creating that file first if the project is new. Never change which project is active by editing a file's contents; only repointing the symlink does that.

## `.vibe_to_spec.yaml` — the project's config

The file the symlink resolves to holds three fields:

```yaml
artifacts: <absolute path or link to the folder holding all five STEP*.md files>
dirty_impl_resources:
  - path: <absolute path or link>
    description: <short phrase identifying what this path contains>
clean_impl_resources:
  - path: <absolute path or link>
    description: <short phrase identifying what this path contains>
```

- **`artifacts`** — where this project's `STEP1_VIBE_DECISIONS.md` … `STEP5_IMPL_VERIFICATION.md` live, all five flat in one folder, with no `step_NN_.../` nesting. Every step's own `CLAUDE.md`, `README.md`, and commands refer to this location as `<artifacts>` — for example `<artifacts>/STEP1_VIBE_DECISIONS.md`.
- **`dirty_impl_resources`** — where the step 1 prototype lives. A list, because a single prototype can span more than one folder or repository (for example, a frontend app and a backend API server); each entry is a `path` (a link or an absolute path) plus a short `description` of what is at that path.
- **`clean_impl_resources`** — the same shape, for the step 4 production implementation.

**Every `path` value, and `artifacts` itself, MUST be an absolute path when it is not a link — never a relative path.** A session can be started from any step's folder, so a relative path would resolve against whichever folder happened to be the working directory, silently pointing at a different location depending on where the session began.

This file is the single, authoritative record of where a project's external code lives. It replaces the older `Prototype:` line that used to live in `STEP1_VIBE_DECISIONS.md` and the `Implementation:` line that used to live in `STEP4_IMPL_SPEC_GAPS.md` — those lines no longer exist in this methodology. Do not look for them, and do not write them.

## Creating a new project

If `projects/<project-name>.vibe_to_spec.yaml` does not exist yet, ask the user:

1. Where should this project's artifacts folder live? (a new or existing folder to hold the five `STEP*.md` files, as an absolute path or a link)
2. Where does the prototype live, if it exists yet? (one or more absolute paths or links, each with a short description) — leave `dirty_impl_resources` empty if step 1 has not started.
3. Where does the production implementation live, if it exists yet? (same shape) — leave `clean_impl_resources` empty before step 4 starts.

Write the answers to `projects/<project-name>.vibe_to_spec.yaml`, then create or repoint `.active_project.vibe_to_spec.yaml` to it.

Both `projects/*.vibe_to_spec.yaml` and the `.active_project.vibe_to_spec.yaml` symlink are git-ignored: they hold paths specific to one user's machine, never committed to this checkout.
