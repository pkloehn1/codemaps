# Renovate

## Detect

`renovate.json`, `renovate.json5`, any `.renovaterc*`, or a `config.js` that exports Renovate options.

## Entry points

`renovate.json` for the repository's own config; `config.js` for the global config a self-hosted runner reads through `RENOVATE_CONFIG_FILE`.

## Look for

- Which config this is: global (a runner's `config.js`, read for every autodiscovered project) or repository (merged over the global one).
- What the global config already provides: `extends`, labels, the commit format, manager opt-ins (`pre-commit`), custom managers over `.gitlab-ci.yml`, package rules.
- The merge rule: a repository option replaces the global value; `customManagers`, `packageRules` and `extends` are added after the global ones, so a restated manager runs twice.
- Each custom manager: `managerFilePatterns`, the `matchStrings` capture groups, the datasource and registry, and whether its pattern matches a file in the tree.
- Package rules and the packages or managers each names.
- `commitBody` and the Dependency Dashboard issue it names; the schedule and limits.
- The validator: `renovate-config-validator` at commit, run bare where `config.js` exists and with `--no-global` where only `renovate.json` does.
- Tests that hold the boundary between global and repository config.

## The map says

`dependencies.md`:

- a Data Flow paragraph stating that two configs cover the repository
- what `renovate.json` holds: the dashboard footer, the repository's own managers and rules, the schedule
- what the global config holds, linked to the runner repository
- every pin location with the manager that tracks it
- the validator at commit, and the version its `rev:` names

`architecture.md`, for the runner repository:

- the scheduled job, what it autodiscovers, that it reaches projects without a config, and the variables it needs

## Dead state

A manager whose pattern matches no file. A package rule naming a package no manager extracts. A disabled manager still named in a rule.

A repository option the global config already sets, which silently wins over the next change there.

## Refresh after

A manager or rule added or removed, an option moving between `renovate.json` and the global config, or a validator hook added.
