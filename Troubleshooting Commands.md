---
tags: []
---

Stop/start something from auto reconciling
```
flux suspend kustomization infra-database-kustomize-git-mysql
flux resume kustomizaion
```

Tell flux to reconcile now
```flux reconcile [type] [thing]```

Get flux kustomizations
```flux export kustomizations --all```

## k9s for everything



