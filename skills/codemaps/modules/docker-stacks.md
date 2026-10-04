# Docker Swarm and Compose stacks

## Detect

Any `docker-compose.yml`, `docker-compose.yaml`, `compose.yml` or `compose.yaml`.

## Entry points

`stacks/<stack>/docker-compose.yml` to deploy, or the compose file at the root for a single stack; the config tree the services bind-mount.

## Look for

- One directory per stack and the services each file declares.
- `deploy:` keys (`replicas`, `placement`, `update_config`) mark a Swarm stack; their absence marks a plain Compose project.
- Top-level `networks:`, `secrets:`, `configs:` and `volumes:`, and which services attach to or mount each; external networks and secrets created outside the file.
- The static configuration tree services bind-mount (`app-config/<service>/`), and which directories no service mounts.
- `x-` anchors shared across services.
- Images and their tags, per stack; versions pinned somewhere other than an `image:` line (a plugin version in a command flag) and the custom manager that reaches them.
- How a stack reaches the cluster: `docker stack deploy` by an operator, a CI job, or a sync check that reports drift.
- Health and smoke checks that read the running stack.

## The map says

`architecture.md`:

- a stack-to-services table, exhaustive, naming `git ls-files stacks` as its universe
- the config tree and which services read it
- the deploy path, and what, if anything, CI deploys
- the drift or smoke tooling that reads the cluster

`dependencies.md`:

- service images per stack, tracked by the `docker-compose` manager
- versions outside `image:` lines, and the custom manager for each
- images under a rule that holds majors back, as a pointer to the rule rather than its reasoning

## Dead state

A service no stack deploys. A config directory no service mounts. A network, secret or config declared that no service uses. A stack file for a stack the cluster no longer runs.

## Refresh after

A stack or service added or removed, a config directory added, a network or secret moved to external, or a deploy path changing.
