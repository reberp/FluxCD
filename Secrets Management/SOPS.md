---
tags: []
date: 2023-10-19
---
# Mozilla Secrets Operations 
Supports tons of vault storage thinks like Vault and AWS/Azure vaults. 
Using GPG example. 


## Setup

Create and Export key
```sh
# Create key
gpg --batch --full-generate-key [args]

# Export public
gpg --export --armor [id]

# Export private
gpg --export-secret-keys --armor [id]

#install SOPS
# wget and install
```

## Position keys

Write private key to kubernetes secret and push public to repo
```sh
kubectl create secret generic sops-gpg --namespace=flux-system --from-file=sops.asc=sops-gpg.key 
#git push public key file to repo
#good practice to delete key
rm sops-gpg.key
gpg --delete-secret-and-public-keys
```

## Use

```sh
#secret-mysql.yaml is not encrypted

#as developer without keys now, pull from repo
gpg --import sops/sops-gpg.pub

# encrypt plaintext secret, data or stringdata fields, replaces file
sops --encrypt --encrypted-regex="^(data|stringData)$" --pgp <id> --in-place secret.mysql.yaml 
```
```yaml
# Kustomization getting the secret yaml and applying needs to know to decrypt it, add to the spec:
decryption:
  provider: sops
  secretRef:
    name: sops-gpg
```
