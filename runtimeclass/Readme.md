# spin installation
```
curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash
sudo mv spin /usr/local/bin/

```
## Project Scaffolding
```
spin kube scaffold --from ghcr.io/spinkube/containerd-shim-spin/examples/spin-rust-hello:v0.13.0 > hello-world.yaml
```
## cert Manager
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.5/cert-manager.yaml

```
## Kwasm

```
helm repo add kwasm http://kwasm.sh/kwasm-operator/
helm install kwasm-operator kwasm/kwasm-operator --namespace kwasm --create-namespace \
  --set kwasmOperator.installerImage=ghcr.io/spinkube/containerd-shim-spin/node-installer:v0.18.0
kubectl annotate node --all kwasm.sh/kwasm-node=true

```

## Spinkube CRD and runtime class
```
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.4.0/spin-operator.crds.yaml
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.4.0/spin-operator.runtime-class.yaml

```
## Spin operator install
```
helm install spin-operator --namespace spin-operator --create-namespace --version 0.4.0 --wait \
  oci://ghcr.io/spinkube/charts/spin-operator

```
## Shim executor 

```
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.4.0/spin-operator.shim-executor.yaml

```

## Deploy app

```
kubectl apply -f https://raw.githubusercontent.com/spinkube/spin-operator/main/config/samples/simple.yaml
kubectl port-forward svc/simple-spinapp 8083:80

```
