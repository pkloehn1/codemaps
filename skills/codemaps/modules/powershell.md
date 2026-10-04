# PowerShell

## Detect

Any `*.ps1`, `*.psm1` or `*.psd1`.

## Entry points

The `.ps1` scripts an operator runs; each `.psm1` module root; `PSScriptAnalyzerSettings.psd1` where it sits.

## Look for

- Modules (`.psm1`) and what imports each: `Import-Module`, `#Requires -Modules`, dot-sourcing, or a manifest's `RequiredModules`.
- Manifests (`.psd1`) and what they export; a module with no manifest exports everything.
- The entry scripts, and the order they run in when one bootstraps another.
- When the repository is reducing its PowerShell, why each file stays: a Windows API, a step before any other runtime exists, a console UI in the same process.
- Pester tests: where they sit, the naming rule discovery depends on, and the Pester version floor.
- The analyzer settings file, where it sits, and what reads it (MegaLinter, a hook, a test). Note when it has to stay at the root for a tool to find it.
- Work tracked to retire or port files, named by issue, so the map states what stays.

## The map says

`architecture.md`:

- a table of the modules and scripts that stay, with why each cannot be the primary language
- the entry script and what it bootstraps
- the files being reduced, named by the issue that owns the reduction rather than listed

`dependencies.md`:

- the required modules and the PowerShell version floor
- the analyzer settings file and the tools that read it
- the Pester floor

## Dead state

A module no script imports. A function exported and never called. A test directory with no tests matching the discovery rule. A script that hardcodes a platform the repository no longer builds.

## Refresh after

A module added or removed, an entry script renamed, or a retirement issue closing.
