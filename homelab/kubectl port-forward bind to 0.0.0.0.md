---
created: 2023-12-21T23:55
updated: 2023-12-21T23:57
---
```
kubectl port-forward svc/[service-name] -n [namespace] [external-port]:[internal-port] --address='0.0.0.0'
```

Just specify the `--address`!

So for example:

```
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address='0.0.0.0'
```