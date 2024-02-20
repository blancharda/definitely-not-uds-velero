# uds-package-velero

Bigbang [Velero](https://repo1.dso.mil/big-bang/product/packages/velero) deployed via flux by zarf

## Deployment Prerequisites

### Resources

- k3d installed on machine

#### General Usage

- Create `velero` namespace
- Label `velero` namespace with `istio-injection: enabled`

#### Object Storage

S3 compatible object storage must be available in order to use this package. Bucket information and access credentials can be provided via configuration values / env vars:

- Bucket ID: `ZARF_VAR_VELERO_BUCKET`
- Bucket Region: `ZARF_VAR_VELERO_BUCKET_REGION`
- Bucket Provider URL: `ZARF_VAR_VELERO_BUCKET_PROVIDER_URL`
- Access Key: `ZARF_VAR_VELERO_BUCKET_KEY`
- Access Key Secret: `ZARF_VAR_VELERO_BUCKET_KEY_SECRET`

## Deploy

### Use zarf to login to the needed registries i.e. registry1.dso.mil

```bash
# Download Zarf
make build/zarf

# Login to the registry
set +o history

# registry1.dso.mil (To access registry1 images needed during build time)
export REGISTRY1_USERNAME="YOUR-USERNAME-HERE"
export REGISTRY1_TOKEN="YOUR-TOKEN-HERE"
echo $REGISTRY1_TOKEN | build/zarf tools registry login registry1.dso.mil --username $REGISTRY1_USERNAME --password-stdin

set -o history
```

### Build and Deploy Everything locally via UDS tasks

```bash
# build the bundle for testing
uds run create-test-bundle

# setup a k3d test env
uds run setup-test-cluster

# deploy the bundle
uds run deploy-test-bundle
```

## Declare This Package In Your UDS Bundle

Below is an example of how to use this projects zarf package in your UDS Bundle

```yaml
kind: UDSBundle
metadata:
  name: example-bundle
  description: An Example UDS Bundle
  version: 0.0.1
  architecture: amd64

packages:
  # Velero
  - name: velero
    repository: ghcr.io/defenseunicorns/uds/velero
    ref: x.x.x
```
