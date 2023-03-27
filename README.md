# cloud-dns

## Description
sample description

## Usage

### Fetch the package
`kpt pkg get REPO_URI[.git]/PKG_PATH[@VERSION] cloud-dns`
Details: https://kpt.dev/reference/cli/pkg/get/

### View package content
`kpt pkg tree cloud-dns`
Details: https://kpt.dev/reference/cli/pkg/tree/

### Apply the package
```
kpt live init cloud-dns
kpt live apply cloud-dns --reconcile-timeout=2m --output=table
```
Details: https://kpt.dev/reference/cli/live/
