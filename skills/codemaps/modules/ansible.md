# Ansible

## Detect

`ansible.cfg`, any `roles/<name>/tasks/main.yml`, or any `playbooks/*.yml`.

## Entry points

The playbooks run per host class (`provision-<class>.yml`, or `site.yml`); `ansible.cfg` for `roles_path` and `inventory`.

## Look for

- `ansible.cfg`: where roles and inventory resolve, and settings that are load-bearing for something else (`pipelining` against `noexec` mounts, for one).
- Inventory: the files, the groups, `group_vars/` and `host_vars/`, and the vault files.
- Roles grouped by what they configure, and the `roles:`, `import_role` and `include_role` lines that invoke each. A role no playbook invokes is the first thing to find.
- Playbooks: one per task, or orchestrating imports, and which carry no `roles:` key.
- `requirements.yml`: the collections declared, the collections the tree uses, and anything that intersects the two before installing.
- Role defaults that pin an application version (`<app>_version`) and the bot manager that reads them.
- Molecule scenarios and the roles they verify; Testinfra or other state checks.
- Playbook `templates/` and `files/` directories, and what renders them.

## The map says

`architecture.md`:

- the per-class entry playbooks
- role groups as a selected table with examples, naming the command for the full set
- the roles no playbook invokes, with the mechanism (a play with no `roles:` key) when there is one
- the Molecule-verified roles

`dependencies.md`:

- `requirements.yml` as the collection manifest, whether it is an install list or a catalogue, and what filters it
- the collections declared ahead of any use
- role defaults carrying versions, and the manager that moves them
- the ansible-core and ansible-lint pins and where they sit

## Dead state

A role no playbook invokes. A collection nothing in the tree references. An inventory group no playbook targets. A playbook with no `roles:` key. A Molecule scenario for a role that no longer exists.

## Refresh after

A role or playbook added or removed, an inventory group added, a collection added to or dropped from `requirements.yml`, or a Molecule scenario added.
