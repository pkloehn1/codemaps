# Contributing

The core (`skills/codemaps/SKILL.md`) is the contract; a module adds one stack. Most contributions are modules.

## Add a module

1. Copy `skills/codemaps/modules/TEMPLATE.md` to `skills/codemaps/modules/<stack>.md`. The name is lowercase and hyphenated, and is how a reader names the stack.
2. Fill every section. Keep the six headings in the template's order; the core reads a module by them.
3. Write it from a real tree. Each Look for line is something you opened; each Dead state line is a state you have seen or can produce.
4. Add one row to the Modules table in `skills/codemaps/SKILL.md`, with the same Detect patterns the module states.
5. Lint, and check the headings, with the two commands below.

```bash
npx markdownlint-cli2 "**/*.md"
grep -c '^## ' skills/codemaps/modules/<stack>.md
```

The second prints `6`.

Then open a pull request. The description names the repository the module was written against, so a reviewer can check it against a tree.

## Change the core

The core changes when a rule is learned by breaking it, the way the existing ones were. The pull request names the map and the failure.

A rule that is true of one stack belongs in that stack's module, not in the core.

## What a pull request does not do

- Add a section to the area-file shape. The shape is ECC's `doc-updater` shape, so maps stay readable by anything that expects it.
- Raise the budget without a measurement. The figure in `skills/codemaps/SKILL.md` carries its derivation; a new figure carries its own.
- Add instruction or explanation to what a map says. A map describes.

## Try it locally

```bash
claude plugin validate .
claude --plugin-dir .
```

See the [README](README.md) for installing the released plugin.
