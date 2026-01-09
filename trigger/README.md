# deployment-trigger

Triggers a deployment workflow using the GitHub CLI. This action initiates a workflow run on a specified reference with the provided build version.

## Inputs

| Name                 | Description                                                              | Required | Default |
|----------------------|--------------------------------------------------------------------------|----------|---------|
| `workflow-path`      | The path to the production workflow file.                                | Yes      | N/A     |
| `workflow-ref`       | The reference (tag or branch) to be used for the workflow run.           | No       | `${{ github.ref_name }}`     |
| `build-version`      | The version to be used for the release.                                  | No       | `''` (empty string) |

## Outputs

| Name                 | Description                                                              |
|----------------------|--------------------------------------------------------------------------|
| `workflow-run-url`   | The URL of the triggered workflow run. Empty if run details cannot be retrieved. |

## Features

- **Automatic Summary**: Generates a job summary with workflow metadata, status, and clickable links
- **Run Verification**: Retrieves and displays the triggered workflow run details
- **Output URL**: Provides the workflow run URL for use in subsequent steps

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
        uses: actions/checkout@v5

      - name: Trigger production deployment
        uses: ./deployment-trigger
        with:
          workflow-ref: main
          workflow-path: .github/workflows/production-deploy.yml
          build-version: v1.2.3

      # Alternative: Use default workflow-ref (cirent branch/tag)
      - name: Trigger production deployment
        uses: ./deployment-trigger
        with:
          workflow-path: .github/workflows/production-deploy.yml
          build-version: v1.2.3
```
