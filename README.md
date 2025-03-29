# GHCR Package Cleanup Action

This GitHub Action automatically cleans up old packages from GitHub Container Registry (GHCR) based on a specified retention period.

## Features

- Deletes container packages older than a specified number of days
- Works with both organization and user repositories
- Optional dry-run mode to preview deletions without performing them
- Displays package information including tags for better identification
- Automatically handles pagination for repositories with many package versions

## Usage

Add this to your GitHub workflow:

```yaml
name: Cleanup GHCR packages

on:
  schedule:
    - cron: '0 0 * * 0'  # Run weekly on Sundays
  workflow_dispatch:     # Allow manual triggering

jobs:
  cleanup:
    runs-on: ubuntu-latest
    permissions:
      packages: write    # Required for package deletion
      
    steps:
      - name: Cleanup old packages
        uses: 404cerebrum/cleanup-ghcr-packages@v1
        with:
          # Required inputs:
          token: ${{ secrets.GITHUB_TOKEN }}
          
          # Optional inputs (shown with defaults):
          # org: ''                          # Organization name (if using org account)
          # username: ${{ github.repository_owner }}  # Username (if using personal account)
          # package-name: ''                 # Defaults to repository name if empty
          # days: '30'                       # Number of days to keep packages 
          # dry-run: 'false'                 # Set to 'true' to only list packages without deleting
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | GitHub token with permissions to delete packages | Yes | `${{ github.token }}` |
| `org` | GitHub organization name | No | `''` |
| `username` | GitHub username (for personal accounts) | No | `${{ github.repository_owner }}` |
| `package-name` | Name of the container package | No | Repository name |
| `days` | Number of days to keep packages | No | `30` |
| `dry-run` | List without deleting if set to `'true'` | No | `'false'` |

## Examples

### Basic usage (organization repository)

```yaml
- name: Cleanup old packages
  uses: 404cerebrum/cleanup-ghcr-packages@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    org: my-organization
    days: 30
```

### Basic usage (personal repository)

```yaml
- name: Cleanup old packages
  uses: 404cerebrum/cleanup-ghcr-packages@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    days: 30
```

### With custom package name

```yaml
- name: Cleanup old packages
  uses: 404cerebrum/cleanup-ghcr-packages@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    org: my-organization
    package-name: my-custom-package
    days: 60
```

### Dry run mode (preview only)

```yaml
- name: Preview package cleanup
  uses: 404cerebrum/cleanup-ghcr-packages@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    days: 14
    dry-run: 'true'
```

## Permissions

This action requires `packages: write` permissions to delete packages. Make sure your workflow includes the appropriate permissions:

```yaml
permissions:
  packages: write
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
