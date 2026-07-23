# Projects

This folder holds one `.vibe_to_spec.yaml` config file per project — `projects/<project-name>.vibe_to_spec.yaml`. Each file records where that project's artifacts folder, prototype, and production implementation actually live, as described in the [root `CLAUDE.md`](../CLAUDE.md).

These files hold paths specific to one user's machine, so they are git-ignored (see `.gitignore` at the repository root) and never committed to this checkout. This checkout stays a pure methodology template; nothing project-specific is ever added to it.

[`template.vibe_to_spec.yaml-sample`](template.vibe_to_spec.yaml-sample) shows the shape to copy from. Its `-sample` suffix keeps it out of the `.gitignore` pattern above, so it stays committed as a reference.
