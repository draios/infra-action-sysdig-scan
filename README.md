# infra-action-sysdig-scan

GitHub Action to scan container images with Sysdig scan

## Inputs

### Required

- `image_tag`: The tag of the local image to scan. Example: "sysdiglabs/dummy-vuln-app:latest"
- `sysdig-secure-token`: API token for Sysdig Scanning auth.
- `scan-type`: "Type of scan to perform. Possible values: "legacy","new". Default: "legacy"

## Optional

- `sysdig-secure-url`: Sysdig Secure URL. Example: https://secure-sysdig.svc.cluster.local If not specified, it will default to Sysdig Secure SaaS URL (https://secure.sysdig.com).
- `sysdig-skip-tls`: Skip TLS verification when calling secure endpoints.
- `ignore-failed-scan`: Don't fail the execution of this action even if the scan result is FAILED.
- `severity`: "Severity level to fail the build"
- `input-type`: If specified, where should we scan the image from.


## Example workflow

Perform all checks on pull requests

```yaml

```
