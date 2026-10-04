# Python packages and scripts

## Detect

`pyproject.toml`, `setup.cfg`, or any `*.py`.

## Entry points

`pyproject.toml` for the package, its scripts and its tool settings; the package root (`src/<pkg>/`, or the top-level package in a flat layout); `<pkg>/__main__.py` where one exists.

## Look for

- The layout: `src/` or flat, and one package or several (`scripts/<pkg>`, each with its own `__init__.py`).
- Whether the package is installed into the venv or imported from the checkout.
- `[project.scripts]`: every console script and the module each resolves to. A hook manifest or CI job that names a script depends on this table.
- `[project.dependencies]`, `[project.optional-dependencies]` and `requires-python`.
- Tool settings in `pyproject.toml` or beside it: ruff, mypy, pytest, coverage, and which tools have no config at all.
- Where tests live (`tests/`, `<pkg>/tests/`) and what gates them: a per-package `coverage.toml` and floor, a CI pytest job, a hook that runs the suites a commit touches.
- Packages imported across the tree (a `scripts/common`, a `_git.py`) and the interpreter shim every hook routes through.
- Shell scripts beside the Python, and which of them something still invokes.

## The map says

`architecture.md`:

- a selected table of packages with purpose and what a session reaches for, naming `git ls-files <root>` for the full set
- the shared primitives package, whatever its size
- the shim or entry script every hook routes through
- where tests sit and the floor they run under
- the gates in order: commit, push, merge request

`dependencies.md`:

- the runtime dependencies and every extra, as a table when there is more than one
- the Python floor, as `requires-python`
- the bot manager that tracks each: `pep621`, `poetry`, `pip_requirements`
- a git-reference pin inside an extra, and the custom manager that moves it
- the tool configs that stay at the root because their tools look only there

## Dead state

A module no script, hook, test or `[project.scripts]` entry imports or names. A console script whose target module is gone.

A package whose tests run outside the coverage floor. A directory that survives only as a `__pycache__` shell. A shell script nothing invokes.

## Refresh after

A package added or removed, a console script added or renamed, tests moving between `tests/` and `<pkg>/tests/`, a coverage floor added, or a dependency moving between `dependencies` and an extra.
