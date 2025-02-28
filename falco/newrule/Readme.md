## Install Falco 
```
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
helm install falco falcosecurity/falco -n falco --create-namespace
```

## Cutom Falco Rule `custom-falco-rules.yaml`
```
- rule: Detect /dev/mem Access
  desc: Detect processes that attempt to read /dev/mem
  condition: (evt.type=open or evt.type=openat) and fd.name=/dev/mem
  output: "Process %proc.name accessed /dev/mem (command=%proc.cmdline user=%user.name container=%container.id image=%container.image.repository)"
  priority: WARNING
  tags: [security]

```
## Apply the rule

```
kubectl create configmap falco-custom-rules --from-file=custom-falco-rules.yaml -n falco
kubectl patch daemonset falco -n falco --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/volumes/-",
    "value": {
      "name": "custom-rules",
      "configMap": {
        "name": "falco-custom-rules"
      }
    }
  }
]'

kubectl patch daemonset falco -n falco --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/containers/0/volumeMounts/-",
    "value": {
      "name": "custom-rules",
      "mountPath": "/etc/falco/rules.d/custom-falco-rules.yaml",
      "subPath": "custom-falco-rules.yaml"
    }
  }
]'
kubectl rollout restart daemonset falco -n falco

```

## check Falco logs

```
kubectl logs -n falco -l app.kubernetes.io/name=falco --tail=50 -f

```
