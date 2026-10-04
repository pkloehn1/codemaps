# Terraform

## Detect

Any `*.tf`.

## Entry points

Each root module (`terraform/environments/<env>/<area>/`, or the repository root); `terraform/modules/<name>/` for the child modules.

## Look for

- Root modules: the directories with a `backend.tf` or `providers.tf`, and the state backend each uses.
- Child modules under `modules/`, and which root calls each with a `module` block.
- `versions.tf` in every root and module: the Terraform constraint and each provider constraint.
- `variables.tf` and `outputs.tf` per module, and the `terraform.tfvars` or `*.auto.tfvars` that carry environment values.
- `.terraform.lock.hcl` presence per root.
- Directories under `terraform/` that hold no `.tf` file at all.
- What runs `plan` and `apply`: a CI job, a service account, or an operator by hand.

## The map says

`architecture.md`:

- a table of every Terraform path with its state (live root, the only module, no Terraform), because a directory count overstates what exists
- what applies it

`dependencies.md`:

- each provider with every path that constrains it, as a table
- where `versions.tf` sits, as a glob that matches the real depth
- whether lockfiles exist
- the bot manager that tracks providers (`terraform`), and what it does not reach

## Dead state

A module no root calls. A provider declared with no resources. A directory under `terraform/` that holds no Terraform. A variable no root sets and no module reads.

## Refresh after

An environment or module added or removed, a provider added, or a backend moved.
