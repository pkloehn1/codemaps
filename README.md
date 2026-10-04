# codemaps

A Claude Code plugin whose skill writes and refreshes `docs/CODEMAPS/` for a repository.

Three Markdown maps that a session reads before exploring an unfamiliar area, so it starts from a map instead of a fresh survey.

One core carries the contract. One module per stack says what to look for and what the map must say for that stack. The core detects the stacks in the tree and loads only those modules.

## Install

```text
/plugin marketplace add pkloehn1/codemaps
/plugin install codemaps@codemaps
```

The skill is invoked as `codemaps:codemaps`. Claude Code picks it up on the next session.

To try it without installing:

```bash
claude --plugin-dir /path/to/codemaps
```

## Update

```text
/plugin marketplace update codemaps
/plugin update codemaps@codemaps
```

Restart Claude Code to apply the update.

The plugin carries no `version`, so Claude Code versions it by commit: every merge to `main` is an update.

## Releases

Releases are tags, cut by the [`release` action](https://github.com/paragon-stats/github-actions/tree/main/release) paragon-stats uses.

On each push to `main`, python-semantic-release reads the Conventional Commits since the last `vX.Y.Z` tag.

| Type | Release |
| --- | --- |
| `feat` | minor |
| `fix`, `perf`, `security`, `revert` | patch |
| anything else | none |

A release-cutting type must change something under `skills/`; pull requests fail `commitlint` otherwise.

The tag is GPG-signed, the GitHub Release lists the changes, and nothing is committed back to `main`.

The bump policy is `[tool.semantic_release]` in `pyproject.toml`, held to the shared `commit-types.txt` by `commitlint`.

## What a map is

Three files under `docs/CODEMAPS/`: `INDEX.md`, `architecture.md`, `dependencies.md`.

Each area file has the same section shape, stays under a stated token budget, describes rather than instructs, and names only paths that exist in `git ls-files`.

The contract is [skills/codemaps/SKILL.md](skills/codemaps/SKILL.md); nothing restates it.

## Stacks

| Module | Stack |
| --- | --- |
| `python` | Python packages and scripts |
| `ansible` | Roles, playbooks, inventory, collections |
| `terraform` | Root modules, child modules, providers |
| `powershell` | Scripts, modules, manifests, Pester |
| `docker-stacks` | Docker Swarm and Compose stacks |
| `gitlab-ci` | GitLab CI/CD components and pipelines |
| `renovate` | Repository and global Renovate config |
| `pre-commit-hooks` | Hook packages and their consumers |
| `dotnet` | Solutions, projects, central package management |

A stack with no module is mapped from the core alone. Adding the module is a pull request: see [CONTRIBUTING.md](CONTRIBUTING.md).

## Why a skill, not the generic command

ECC's `/ecc:update-codemaps` is written for JavaScript and TypeScript projects.

Run as written it creates three files that describe nothing in an infrastructure repository, caps a map below what an honest one needs, and date-stamps files whose content did not change.

`SKILL.md` lists each correction.

## Provenance

The contract was first a section of a homelab repository's standards, and a skill copied into two repositories. The copies drifted. This repository is the one source; those repositories now link here.

## License

MIT
