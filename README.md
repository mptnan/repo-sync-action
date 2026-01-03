# Sync Repository Action

A GitHub Action to sync a source repository to the current repository, creating a new branch without workflow files.

## Features

- Sync from any GitHub repository (public or private)
- Create a branch with commit hash suffix (e.g., `synced-abc123`)
- Automatically excludes `.github/workflows/` directory
- Support for private repositories via token authentication
- Dry run mode for testing

## Usage

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    permissions:
      contents: write  # Required for pushing branches
    steps:
      - uses: mptnan/repo-sync-action@v1.0.0
        with:
          source_repo: 'owner/repo'  # Format: owner/repo
          source_branch: 'main'
          branch_prefix: 'synced'
          source_commit: ''  # Optional: specific commit hash from source
          dry_run: false
          fetch_token: ${{ secrets.MY_PAT }}  # Optional: for fetching from private source repos
          push_token: ${{ secrets.MY_PAT }}  # Optional: for pushing branches (if different from fetch_token)
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `source_repo` | Source repository in format `owner/repo` | Yes | - |
| `source_branch` | Source branch name | No | `main` |
| `source_commit` | Source commit hash to sync to (optional) | No | Latest |
| `branch_prefix` | Output branch name prefix | No | `synced` |
| `dry_run` | Dry run mode (no push) | No | `false` |
| `fetch_token` | GitHub token for fetching from source repository | No | Default GITHUB_TOKEN |
| `push_token` | GitHub token for pushing branches | No | Default GITHUB_TOKEN |
| `git_user_name` | Git user name for commits | No | `github-actions[bot]` |
| `git_user_email` | Git user email for commits | No | `github-actions[bot]@users.noreply.github.com` |

## Examples

### Basic usage

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    permissions:
      contents: write  # Required for pushing branches
    steps:
      - uses: mptnan/repo-sync-action@v1.1.0
        with:
          source_repo: 'octocat/Hello-World'
          source_branch: 'main'
          branch_prefix: 'synced'
```

### Sync from private repository

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: mptnan/repo-sync-action@v1.1.0
        with:
          source_repo: 'owner/private-repo'
          fetch_token: ${{ secrets.MY_PAT }}  # Required for private source repos
```

### Dry run mode

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: mptnan/repo-sync-action@v1.1.0
        with:
          source_repo: 'owner/repo'
          dry_run: true
```

## Notes

- **Permissions**: This action requires `contents: write` permission to push branches. Add `permissions: contents: write` to your job if your repository uses default read-only permissions.
- `fetch_token` is used for fetching from the source repository. If not specified, uses default `GITHUB_TOKEN`.
- `push_token` is used for pushing branches. If not specified, uses default `GITHUB_TOKEN`.
- The `.github/workflows/` directory is automatically removed from the synced branch if it exists. This is necessary because GitHub Actions does not allow workflows to be pushed by GitHub Actions to prevent recursive workflow triggers and security issues.
- Branch names follow the pattern: `{branch_prefix}-{commit_hash}`

## License

This project is licensed under the [MIT License](./LICENSE).

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md).
