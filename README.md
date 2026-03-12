# actions-yarn-run

Composite GitHub Action that handles the full Node.js quality check lifecycle: checkout, cache `node_modules`, configure `.npmrc` for GitHub Packages, install dependencies, and run any `yarn` command.

Replaces the repeated boilerplate across `format`, `typecheck`, `test`, `lint`, `build`, and `audit` jobs.

## Usage

```yaml
- name: Lint
  uses: devops-looplava/actions-yarn-run@main
  with:
    COMMAND: lint
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

- name: Format Check
  uses: devops-looplava/actions-yarn-run@main
  with:
    COMMAND: format
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

- name: Yarn Audit
  uses: devops-looplava/actions-yarn-run@main
  with:
    COMMAND: "audit --severity critical || true"
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `COMMAND` | Yes | - | Yarn command to run (e.g. `format`, `typecheck`, `test`, `lint`, `build`, `audit --severity critical \|\| true`) |
| `GITHUB_TOKEN` | Yes | - | GitHub token for authenticating to GitHub Packages |
| `NODE_VERSION` | No | - | Node.js version (informational, uses runner's installed version) |
| `NPM_SCOPE` | No | `@onoctave` | npm scope for GitHub Packages registry |

## What it does

1. Checks out the repository
2. Restores `node_modules` from cache (keyed on `yarn.lock` hash)
3. Creates `.npmrc` for GitHub Packages auth (if not present)
4. Runs `yarn install --frozen-lockfile`
5. Runs `yarn <COMMAND>`
