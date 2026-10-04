# pre-commit hook packages and consumers

## Detect

`.pre-commit-hooks.yaml` (a hook package) or `.pre-commit-config.yaml` (a consumer).

## Entry points

`.pre-commit-hooks.yaml` for the hooks a package ships; `.pre-commit-config.yaml` for the hooks a repository runs; the packaged copy of the manifest, where one exists.

## Look for

- A package: every manifest entry, the console script or module its `entry` resolves to, its `stages`, `files` and `types`, and ids kept as aliases for older configs.
- A packaged copy of the manifest, and the test that holds the two equal.
- Shared primitives the hook modules import (git index reads, YAML with pipeline tags, an issue-reference grammar) and data files they read (a commit-types list).
- A consumer: the hook repositories by `rev:`, `repo: local` hooks and the interpreter shim they route through, and third-party hooks.
- Each third-party hook's `language`: a `node` hook makes CI download Node, which needs `libatomic1`.
- `default_install_hook_types` and `default_stages`, and which stage each hook runs at.
- Whether something other than pre-commit owns a git hook (Git LFS on pre-push) and how the two are combined.
- The register of local hooks and overridden arguments, and the check that holds it against the config.
- A self-test or CI job that installs the package the way a consumer's `repo:` entry would.

## The map says

`architecture.md`, for a package:

- a selected module table, naming the command for the full set, with the shared primitives and data files whatever their size
- the aliases

`architecture.md`, for a consumer:

- which hooks are shared, local or third-party
- the gate order, with the stage each hook runs at
- who owns each git hook when pre-commit does not

`dependencies.md`:

- `rev:` pins, and the manager that tracks them (the `pre-commit` manager, opted into)
- local hooks that carry no pin
- the runtimes a hook needs (Node, Docker) and what installs them in CI

## Dead state

A hook module no manifest entry names. A manifest entry whose script is gone. An exemption or register entry naming a hook the config no longer has.

A stage in `default_install_hook_types` nothing runs at. A configured hook that matches no file type in the tree.

## Refresh after

A hook added or removed, a hook moving between local and shared, a stage added, or `default_install_hook_types` changing.
