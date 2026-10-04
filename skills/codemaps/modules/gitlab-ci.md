# GitLab CI/CD components and pipelines

## Detect

`.gitlab-ci.yml`, any `templates/<name>/template.yml`, or any `templates/<name>.yml`.

## Entry points

`.gitlab-ci.yml` for the gates; `templates/<name>/template.yml` for each component a catalog repository ships.

## Look for

- `include:` entries: `component:` at a tag, `local:` paths, `ref:` inputs, and each include's `inputs.image`.
- `stages:`, `workflow:` rules, and the jobs declared in the file itself against those a component brings.
- Each job's `rules:` and when it runs: merge request, default branch, schedule, tag.
- A catalog repository: `spec:inputs` per `template.yml`, the README beside it, the `release` job, and that only a tag publishes.
- A self-test job that installs the checkout the way a consumer would.
- `.shared-check-exemptions.jsonc` or its equivalent: the jobs the repository declares itself and why.
- CI/CD variables a job needs (`GITLAB_API_TOKEN`, a bot token) and which jobs do not run without them.
- A pin another tool reads back from the pipeline (a local hook reading the MegaLinter image from the include).

## The map says

`architecture.md`:

- the gates in order, commit, push, merge-request pipeline, with the job each runs
- a job table with defined-by and runs-on
- what a merge publishes and what a tag publishes
- for a catalog repository, every component with the job it ships and when it runs, exhaustive against `git ls-files templates`

`dependencies.md`:

- component versions and `ref:` inputs
- include images and job images as separate rows, because different managers read them
- the registry a component resolves against
- the tag another tool reads back, so it exists once

## Dead state

A job no rule lets run. A stage no job uses. An include at a `ref:` nothing resolves. A component no consumer includes. An exemption naming a job the file no longer declares.

## Refresh after

A job, stage or component added or removed, a pin moving to another file, or a variable becoming required.
