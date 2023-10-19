sigstore for signing and verifying with cosign tool
Flux comes with it

## Install and Setup

wget binary from github
Generate keys and put into flux
```
cosign generate-key-pair
kubectl -n flux-system create secret generic cosign-pub --from-file=cofign.pub=cosign.pub
```


## Use

login to docker and push artifact
```sh
docker login ghcr.io --username --password
flux push artifact oci://ghcr.io/reberp/bb-app:7.7.0-$(git rev-parse --short HEAD) --path="./manifests" --source="$(git config --get remote.origin.url)" --revision="7.7.0/$(git rev-parse --short HEAD)"
```

Sign and push signature
```sh
cosign sign --key cosign.key ghcr.io/reberp/bb-app@sha256:[sha256]
```
Can see that signature is pushed to repo 

verify a pulled file
```sh
cosign verify --key cosign.pub ghcr.io/reberp/bb-app@sha256:[sha256]
```

### Incorporate into Flux
```sh
flux create source oci 10-demo-source-oci-bb-app --url oci://ghcr.io/reberp/bb-app --tag 7.10.0-[hash] --secret-ref ghcr-auth --provider generic --export > 10-demo-source-oci-bb-app.yaml
# add verify, provider secretRef section with name cosign-pub
flux create kustomization 10-demo-kustomize-oci-bb-app --source OCIRepository/10-demo-source-oci-bb-app --target-namespace 10-demo --interval 10s --prune false --export > 10-demo-kustomize-oci-bb-app.yaml
# commit both files and wait 
```