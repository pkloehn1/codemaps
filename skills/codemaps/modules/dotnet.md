# .NET

## Detect

Any `*.sln`, `*.slnx`, `*.csproj` or `*.fsproj`, or `Directory.Build.props`.

## Entry points

The solution file (`<Name>.slnx` or `<Name>.sln`); the executable project (`src/<Name>.Cli/`); `Directory.Build.props` for what every project inherits.

## Look for

- The solution's folders and projects, and each project's kind: class library, console, analyzer, test.
- `ProjectReference` edges, including an analyzer reference (`OutputItemType="Analyzer"`) that `Directory.Build.props` injects into every other project.
- `Directory.Build.props`: language version, nullable, warnings as errors, analysis level, analyzer packages, versioning (MinVer and its tag prefix).
- `AdditionalFiles` such as `stylecop.json`, and `.editorconfig` where the analyzers read it.
- `Directory.Packages.props`: central package management, so no `.csproj` carries a version.
- `global.json`: the SDK version, `rollForward`, and the test runner; `dotnet.config` beside it.
- `.config/dotnet-tools.json`: the local tools and which script or hook runs each.
- Test projects, the framework and runner (xUnit v3 on Microsoft.Testing.Platform), the coverage gate and its class filter, golden fixtures.
- The publish shape: target framework, RID, native AOT, and the platform CI verifies.
- Git hooks (Husky.Net `task-runner.json`) and what runs at commit against push; the file-based `scripts/dev/*.cs` apps those tasks run.
- CI workflows, and the one whose check name is required on the default branch.

## The map says

`architecture.md`:

- a project table, exhaustive against the solution file, with kind and references
- the analyzer that loads into every other project, and the project it excludes
- the gates in order (commit, push, CI) with what each runs
- the publish target and the platform verified

`dependencies.md`:

- `Directory.Packages.props` as the one place every NuGet version lives
- `global.json` for the SDK and `rollForward`
- `dotnet-tools.json` for local tools
- the bot that tracks each (Dependabot's `nuget` ecosystem, or Renovate's `nuget` manager), and what neither reaches

## Dead state

A project in no solution. A `PackageVersion` with no `PackageReference`. A local tool no script or hook runs. A workflow no event triggers. A golden fixture no test reads.

## Refresh after

A project added or removed, a reference edge changing, a tool added to `dotnet-tools.json`, or a hook task moving between commit and push.
