# <Stack, as a reader names it>

## Detect

The `git ls-files` patterns that select this module, as file names and globs. Any match selects it. The same patterns go into the Modules table in `SKILL.md`.

## Entry points

What the `Entry Points` line of an area file names for this stack: the file a reader opens first to build, deploy, run or configure it.

## Look for

- One line per thing to survey, with where it lives and what reads it.
- The files that hold a decision another file depends on: a version read back, a setting that is load-bearing elsewhere.
- Where tests live and what gates them.
- Anything imported or invoked across the tree.

## The map says

`architecture.md`:

- what the Architecture, Key Modules and Data Flow sections must state when this stack is present
- which tables are exhaustive, with the command that gives their universe, and which are selected

`dependencies.md`:

- every pin location this stack adds, one row each, and the bot manager that tracks it

## Dead state

What dead means for this stack, as states a reader can check: a thing nothing invokes, references, deploys or imports.

## Refresh after

The structural changes in this stack that make a map stale.
