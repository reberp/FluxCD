---
tags: []
date: 2023-10-19
---
Deployed as a controller
installed kustomize, helm, source
encrypted with kubeseal
sealed when they're pushed, but once pulled it's decrypted and stored normally locally (b64'd)

# Install
```
#Kubeseal
[wget kubeseal, untar]
```

# Setup

To sync with a repository to pull sealed secrets kustomization file. Already set up GitRepo object
```sh
flux create kustomization infra-security-kustomize-git-sealed-secrets --source GitRepository/infra-source-git --prune true --interval 1h --path ./bitnami-sealed-secrets --export

# to get cert, either:
k -n kube-system get secret sealed-secrets-keyw5fwf -o yaml
# can b64decode the .crt
kubeseal --fetch-cert --controller-name sealed-secrets-controller --controller-namespace kube-system
```

# Use
How to encrypt a secret value
```sh
# Delete old secret
k delete secret [the secret object]

# Encrypt secret, generates SealedSecret object
kubeseal -o yaml --scope cluster-wide --cert [pub file from above] < secret-mysql.yaml > sealed-secret-mysql.yaml
```


