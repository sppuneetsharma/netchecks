

## Installation

Warning: Don't actually use this yet!

Install the CRDs and operator with:

```shell
kubectl apply -f manifests/crds
kubectl create namespace netcheck
kubectl apply -f manifests/operator -n netcheck
```

Then apply your `NetworkAssertions` as any other resource.

## Image Verification

### Prerequisites

You will need to install [cosign](https://docs.sigstore.dev/cosign/installation/).

Verify Signed Container Images
```
$ COSIGN_EXPERIMENTAL=1 cosign verify --certificate-github-workflow-repository hardbyte/netcheck-operator --certificate-oidc-issuer https://token.actions.githubusercontent.com ghcr.io/hardbyte/netcheck-operator:main | jq
```

### Note

`COSIGN_EXPERIMENTAL=1` is used to allow verification of images signed in KEYLESS mode. To learn more about keyless signing, please refer to [Keyless Signatures](https://github.com/sigstore/cosign/blob/main/KEYLESS.md#keyless-signatures).

To verify that an image was created for a specific release add the following to the cosign command:

--certificate-github-workflow-ref refs/tags/[RELEASE TAG] ghcr.io/hardbyte/netcheck-operator:[VERSION] | jq

## Development

### Start a test cluster

```shell
kind create cluster
kubectl config use-context kind-kind
```


### Start the operator

```shell
kopf run main.py --verbose
```
