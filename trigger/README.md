# deployment-trigger

Triggers a deployment workflow using the GitHub CLI. This action initiates a workflow run on a specified reference with the provided build version.

## Inputs

| Name                 | Description                                                              | Required | Default |
|----------------------|--------------------------------------------------------------------------|----------|---------|
| `workflow-ref`       | The reference (tag or branch) to be used for the workflow run.           | Yes      | N/A     |
| `workflow-path`      | The path to the production workflow file.                                | Yes      | N/A     |
| `build-version`      | The version to be used for the release.                                  | No       | `''` (empty string) |

## Outputs

This action does not produce any outputs.

## Environment Variables

This action requires the `GITHUB_TOKEN` environment variable to trigger workflows via the GitHub CLI.

## Permissions Required

The action requires the following minimal permissions:
- `actions: write` - To trigger workflow runs
- `contents: read` - To access repository workflow files

## Example Usage
```yaml
jobs:
  trigger-deploy:
    runs-on: linux-nonprod
    permissions:
      actions: write   # Required to trigger workflows
      contents: read   # Required to access workflow files
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        
      - name: Trigger production deployment
        uses: ./deployment-trigger
        with:
          workflow-ref: main
          workflow-path: .github/workflows/production-deploy.yml
          build-version: v1.2.3
```
