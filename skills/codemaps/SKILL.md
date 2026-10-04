---
name: codemaps
description: Writes and refreshes docs/CODEMAPS (INDEX.md, architecture.md, dependencies.md) against one contract - three files, a fixed section shape, a token budget, describe-never-instruct, every path from git ls-files - and loads one module per stack it detects in the tree (Python, Ansible, Terraform, PowerShell, Docker stacks, GitLab CI, Renovate, pre-commit hooks, .NET) for what to look for and what the map must say. Use when asked to write, refresh or review codemaps, after a structural change, or wherever /ecc:update-codemaps would otherwise run.
---

# Codemaps

A codemap says where things are and what they are for, so a session starts from a map instead of a fresh survey.

This file is the contract. A repository's standards link here and add only what is theirs: a naming exemption, where its `CLAUDE.md` carries the pointer, the lint config the procedure runs.

`/ecc:update-codemaps` is a generic prompt written for JavaScript and TypeScript projects. Treat it as the prompt to rescan; this skill decides the output.

## What exists

Three files under `docs/CODEMAPS/`, and no more:

| File | Contents |
| --- | --- |
| `INDEX.md` | The area table, the scope boundary, how a refresh runs, and what is not enforced |
| `architecture.md` | What the repository contains and how a change reaches production |
| `dependencies.md` | Every pin location, one class of dependency each, and what tracks it |

ECC's template also names `backend.md`, `frontend.md` and `data.md`. A repository with no API routes, no component tree and no application database does not create them.

Its `INDEX.md` says so, which makes their absence read as a decision rather than an omission.

`INDEX.md` holds, in this order:

- the area table: one row per map and what it covers, never a summary of its contents
- the three absent files, and why
- the scope: reference material, and where procedure and rationale live instead
- how to refresh: through this skill, not the generic command
- what is not enforced

## Section shape

Each area file opens with `Last Updated` and `Entry Points`, then Architecture, Key Modules, Data Flow, External Dependencies, Related Areas.

Table columns adapt to what the area holds. The headings do not.

## Writing rules

- **Describe, never instruct or explain.** Procedure belongs in a README or runbook, rationale in a decision record or the comment beside the setting. A map that explains has become something else.
- **Every path named must exist.** Write from `git ls-files`, not `ls`: a directory emptied by `git rm` survives on disk as a `__pycache__` shell and reads as a live package.
- **State what is dead.** Each module names what dead means for its stack. Name the state, not the history behind it.
- **Say whether a list is exhaustive or selected**, and name the command that gives the full set. Prefer selected: an exhaustive table is a second copy of a command's output, which drifts and costs budget.
- **Never close a completeness gap with a count.** A count is wrong from the next commit.
- **Anything imported or invoked across the tree appears in the map**, whatever its size. A shared primitives package belongs at any size; a directory holding one shell script does not.
- **Where membership is a decision rather than a query, name the decision.** A list of what survives a refactor is defined by that refactor, not by a path glob.
- **One paragraph per thought, on one line.** Do not wrap prose to fit a limit; rewrite it shorter. The repository's `MD013` setting is the limit.

## Budget

Each file stays under 1500 tokens, measured as the word count times 1.33, rounded.

```bash
wc -w docs/CODEMAPS/*.md
```

Multiply each count by 1.33. The method is named because two plausible estimators disagree by half and straddle the limit in opposite directions.

1500 is derived, not inherited. ECC's template says 1000, a default written for JavaScript projects.

1500 was measured in kloehnwars-homelab against the survey a map replaces: 268 tracked paths under `scripts/` and 288 under `automation/roles/`.

A cap below what an honest map needs buys a wrong map at full price. There it produced seven of seventeen packages and four false counts.

A map over the cap has a layout problem. Reconsider the area; never cut content to reach a number.

A map that exceeds the budget while the tree it describes is still shrinking records the overage in `INDEX.md` rather than trimming to fit.

## Modules

A module adds, for one stack: what the `Entry Points` line names, what to look for, what the map must say, what counts as dead, and what makes the map stale.

The core selects modules from the tree; read only the ones that match. The `modules/` directory sits beside this file.

| Module | Selected when `git ls-files` has |
| --- | --- |
| [python](modules/python.md) | `pyproject.toml`, `setup.cfg`, or any `*.py` |
| [ansible](modules/ansible.md) | `ansible.cfg`, any `roles/<name>/tasks/main.yml`, or any `playbooks/*.yml` |
| [terraform](modules/terraform.md) | any `*.tf` |
| [powershell](modules/powershell.md) | any `*.ps1`, `*.psm1` or `*.psd1` |
| [docker-stacks](modules/docker-stacks.md) | any `docker-compose.yml`, `docker-compose.yaml`, `compose.yml` or `compose.yaml` |
| [gitlab-ci](modules/gitlab-ci.md) | `.gitlab-ci.yml`, any `templates/<name>/template.yml`, or any `templates/<name>.yml` |
| [renovate](modules/renovate.md) | `renovate.json`, `renovate.json5`, any `.renovaterc*`, or a `config.js` that exports Renovate options |
| [pre-commit-hooks](modules/pre-commit-hooks.md) | `.pre-commit-hooks.yaml` or `.pre-commit-config.yaml` |
| [dotnet](modules/dotnet.md) | any `*.sln`, `*.slnx`, `*.csproj` or `*.fsproj`, or `Directory.Build.props` |

A stack in the tree with no module here is mapped from the rules above. The gap is closed by adding the module: see [CONTRIBUTING.md](https://github.com/pkloehn1/codemaps/blob/main/CONTRIBUTING.md).

## Where the generic command is wrong

| It does | This contract says |
| --- | --- |
| Creates `backend.md`, `frontend.md`, `data.md` | Three files, and no more |
| Writes an HTML comment header | `Last Updated` and `Entry Points` lines |
| Reports to `.reports/codemap-diff.txt` | No such tree exists |
| Imposes its own section shape | ECC's `doc-updater` shape, above |
| Caps a file at 1000 tokens | 1500, derived above |
| Writes prose under no line limit | The repository's `MD013` limit |

`ecc:doc-updater` also ships a `generate.ts` that is not run. It classifies only JavaScript and TypeScript paths and date-stamps every file whether the content changed or not.

## Procedure

1. Write `git ls-files` to a scratch file. Select the modules whose patterns it matches, and read them.
2. Rescan the tree for the areas each map covers, following each selected module's Look for list.
3. Rewrite by hand, against this contract and the selected modules rather than the generic command. Write for the tree as it will be once the branch merges.
4. Re-date only the files whose content changed. A date bump on an unchanged file is the behaviour `generate.ts` is criticised for.
5. Check every path the maps name against the scratch file, and each map's word count against the budget.
6. Lint the Markdown over the whole branch rather than the last edit, with the repository's own markdownlint config. A finding in an earlier commit survives a check scoped to the last edit.
7. Read the diff. A refresh is a reviewed change, not a regeneration.

The check for step 6, with the config path the repository keeps (`.linters/.markdownlint.yml` in the homelab repositories):

```bash
npx markdownlint-cli2 --config <markdownlint config> $(git diff --name-only main...HEAD -- '*.md')
```

## When

After a structural change: a package, role, stack, module, project or component added or removed, a CI job added, or a pin moving to another file.

A Renovate option moving between a repository's config and the global one counts too. Each module names the changes for its stack.

Nothing enforces freshness. The `Last Updated` line is the only signal, and a stale date is a reason to verify rather than a failure.

## The read path

ECC reads no codemap automatically. A repository's `CLAUDE.md` carries the pointer, and that pointer is the entire read path:

> Read `docs/CODEMAPS/INDEX.md` before exploring an unfamiliar area.
>
> Refresh through the `codemaps:codemaps` skill ([pkloehn1/codemaps](https://github.com/pkloehn1/codemaps)) after a structural change and review the diff; nothing enforces freshness.

A repository carries no copy of this skill of its own: a copy drifts.
