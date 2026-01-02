# Sync Repository Action

A GitHub Action to sync a source repository to the current repository, creating a new branch without workflow files.

## Features

- Sync from any GitHub repository (public or private)
- Create a branch with commit hash suffix (e.g., `synced-abc123`)
- Exclude files/directories using glob patterns
- Support for private repositories via token authentication
- Dry run mode for testing

## Usage

```yaml
- uses: your-org/repo-sync-action@v1
  with:
    source_repo: 'owner/repo'  # Format: owner/repo
    source_branch: 'main'
    branch_prefix: 'synced'
    source_commit: ''  # Optional: specific commit hash from source
    dry_run: false
    exclude_patterns: '.github/workflows/**'  # Optional: files to exclude
    github_token: ${{ secrets.MY_PAT }}  # Optional: for private repos
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `source_repo` | Source repository in format `owner/repo` | Yes | - |
| `source_branch` | Source branch name | No | `main` |
| `source_commit` | Source commit hash to sync to (optional) | No | Latest |
| `branch_prefix` | Output branch name prefix | No | `synced` |
| `dry_run` | Dry run mode (no push) | No | `false` |
| `exclude_patterns` | Comma-separated glob patterns to exclude | No | `.github/workflows/**` |
| `github_token` | GitHub token for private repos | No | - |
| `git_user_name` | Git user name for commits | No | `github-actions[bot]` |
| `git_user_email` | Git user email for commits | No | `github-actions[bot]@users.noreply.github.com` |

## Examples

### Basic usage

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/repo-sync-action@v1
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
      - uses: your-org/repo-sync-action@v1
        with:
          source_repo: 'owner/private-repo'
          github_token: ${{ secrets.MY_PAT }}
```

### Custom exclude patterns

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/repo-sync-action@v1
        with:
          source_repo: 'owner/repo'
          exclude_patterns: '.github/workflows/**,*.md,docs/**'
```

### Dry run mode

```yaml
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/repo-sync-action@v1
        with:
          source_repo: 'owner/repo'
          dry_run: true
```

## Notes

- Push always uses the default `GITHUB_TOKEN` (scoped to the repository)
- `github_token` is only used for fetching from private source repositories
- Excluded files are removed before creating the branch
- Branch names follow the pattern: `{branch_prefix}-{commit_hash}`

## License

This project is licensed under the [MIT License](./LICENSE).

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md).
