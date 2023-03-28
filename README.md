# KCC Cloud DNS Example

## Description

This repository is an example of how to implement a kubernetes [Config Connector](https://cloud.google.com/config-connector/docs/overview) based version of the [CDS DNS repository](https://github.com/cds-snc/dns) for managing DNS.

The goal of this is to demonstrate the ability to make a PR against this repository and have [KCC](https://cloud.google.com/config-connector/docs/overview) provision the DNS in a GCP org. Added goals are to validate the configuration of the DNS objects on a PR to ensure that they are compliant. 

he Config Connector resources are managed in a [Config Controller](https://cloud.google.com/anthos-config-management/docs/concepts/config-controller-overview) instance and deployed into a namespace and using Workload Identity the SA can only provision DNS records in a target Project.

### Policy

Policy is implemented using the [kpt](https://kpt.dev/) [Gatekeeper function](https://catalog.kpt.dev/gatekeeper/v0.2/) to run [Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/) policies outside of the cluster and [Policy Controller](https://cloud.google.com/anthos-config-management/docs/concepts/policy-controller) in the Config Controller Instance.

### Implementation

In order to use this example you will need to fork this repo and spin up a config controller instance and configure it to watch the forked repo.

The `root-sync` object should look similar to this example.

```
apiVersion: configsync.gke.io/v1beta1
kind: RootSync
metadata:
  name: dns-root-sync
  namespace: config-management-system
spec:
  sourceType: git
  sourceFormat: unstructured
  git:
    repo: https://github.com/yourusername/cloud-dns
    auth: None # Keep as none if public
```

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
