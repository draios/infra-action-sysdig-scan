# infra-action-sysdig-scan

GitHub Action to scan container images with Sysdig scan

## Inputs

### Required

- `image_tag`: The tag of the local image to scan. Example: "sysdiglabs/dummy-vuln-app:latest"
- `sysdig-secure-token`: API token for Sysdig Scanning auth.
- `scan-type`: "Type of scan to perform. Possible values: "legacy","new", "both". Default: "legacy"

## Optional

- `sysdig-secure-url`: Sysdig Secure URL. Example: https://secure-sysdig.svc.cluster.local
- `sysdig-skip-tls`: Skip TLS verification when calling secure endpoints.
- `ignore-failed-scan`: Don't fail the execution of this action even if the scan result is FAILED.
- `severity`: "Severity level to fail the build"
- `input-type`: If specified, where should we scan the image from.
- `policy-list`: Comma separated list of policies to include


## Example workflow

Perform all checks on pull requests

```yaml
name: Scan image
on: [pull_request]

jobs:

  build:
    runs-on: self-hosted
    steps:
    - uses: actions/checkout@v2
    - uses: draios/infra-action-buildnpush@v1
      with:
        push_to_quay: true
        image_name: "redis-6"
        image_description: "sysdig image for redis-6"
        context_path: "containers/redis"
        dockerfile: "Dockerfile"
        dry_run: ${{ ! ((github.event_name == 'push' && github.ref == 'refs/heads/main') || (github.event_name == 'workflow_dispatch' && github.event.inputs.dry_run == 'false')) }}
        artifactory_username: ${{ secrets.ARTIFACTORY_USERNAME }}
        artifactory_password: ${{ secrets.ARTIFACTORY_PASSWORD }}
        quay_username: ${{ secrets.QUAY_USERNAME }}
        quay_password: ${{ secrets.QUAY_PASSWORD }}
        github_token: ${{ secrets.GITHUB_TOKEN }}

  scan:
    runs-on: self-hosted
    needs: build
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Scan image
        uses: draios/infra-action-sysdig-scan@v1
        with:
          image-tag: "${{ needs.build.outputs.names }}"
          sysdig-secure-token: ${{ secrets.SYSDIG_SECURE_TOKEN }}
          scan-type: "legacy"
          severity: "high"
          ignore-failed-scan: "true"
```
