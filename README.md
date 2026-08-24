# hrm-pipeline-templates

Reusable GitHub Actions workflow templates for HRM Enterprises repositories.

## How it works

This repo holds the **actual pipeline logic** â€” build checks, deploy steps, etc. â€” in one place. Individual repos carry only a thin wrapper file (10â€“15 lines) that points here via `workflow_call`. When the shared logic needs to change, it changes once here and every repo picks it up on its next run.

Do not copy-paste workflow logic into individual repos. Add it here as a reusable workflow, then call it from the wrapper.

---

## Workflows

### `dotnet-pr-checks.yml`
Runs on every pull request. Validates formatting (`csharpier check`) and confirms the project builds (`dotnet build`). Authenticates against GitHub Packages to resolve internal NuGet dependencies (e.g. AppCommonCore).

### `dotnet-deploy.yml` _(coming soon)_
Deploys a published .NET app to an internal server via a self-hosted runner. Requires a runner with network access to `HRMT-APP-02` (test) and `HRM-APP-02` (prod).

---

## Adding a new repo

1. Copy `.github/workflows/pr-checks.wrapper.yml` from `.dev-standards` into your repo's `.github/workflows/` folder.
2. Copy `nuget.config` from this repo's `config/` folder into your repo root.
3. Update the `app-name` input if your repo uses the deploy workflow.

---

## Versioning

Wrapper files should pin to a tag (`@v1`) rather than `@main` so that in-flight changes to this repo cannot break an unrelated deploy mid-run. Tags are cut here when breaking changes are made to the shared workflows.
