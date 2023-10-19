All controllers expose 8080 for metrics

PodMonitor type is part of Flux

To see scrape_configs:
```
kubectl exec -it prometheus -c config-reloader -- sh
cat /etc/prometheus/config_out/prometheus.env.yaml
```

## Prometheus

## Install
Download from github with source and kustomization
```sh
flux create source git monitoring-source-prometheus-stack --interval 30m --url https://github.com/fluxcd/flux2 --branch main --export > monitoring-source-prometheus-stack.yaml

flux create kustomization monitoring-kustomization-prometheus-stack --interval 1h --source monitoring-source-prometheus-stack --path "./manifests/monitoring/kube-prometheus-stack" --export > monitoring-kustomization-prometheus-stack.yaml
```

Change the services to NodePort for grafana and prometheus to see locally 
Secrets are available as secrets on the cluster

### Update Prometheus podmonitor
podmonitor.yaml in monitoring-config folder not installed yet. Add with kustomize. Git source already exists. 
```sh
flux create kustomization monitoring-config --interval 1h --prune true --source monitoring-source-prometheus-stack --path "./manifests/monitoring/monitoring-config" --export > monitoring-config.yaml
```

Go to dashboard and can see podMonitor on targets. 

![[Pasted image 20231019085817.png]]

gotk_reconcile_condition shows on dashboard as having events coming in. Not sure what that object represents. 
## Grafana

Works at this point. Can be used to show metrics like git operations per minute. Success and Fails. Etc. 
![[Pasted image 20231019090442.png]]

