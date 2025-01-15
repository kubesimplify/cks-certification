## Install bom cli
```
wget https://github.com/kubernetes-sigs/bom/releases/download/v0.6.0/bom-amd64-linux
chmod +x bom-amd64-linux 
sudo mv bom-amd64-linux /usr/local/bin/bom
```
## Use bom to generate sbom for controller manager image 
```
bom generate spdx-json \
    --image registry.k8s.io/kube-controller-manager:v1.32.0 \
    --output ./sbom1.json

```

## Install trivy 
```
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh
sudo mv bin/trivy /usr/local/bin

trivy image --format cyclonedx \
    --output ./sbom2.json \
    registry.k8s.io/kube-controller-manager:v1.32.0

```

## Use trivy 
```
trivy sbom  ./sbom1.json     --format json     --output ./sbom_check_result.json
cat sbom_check_result.json | jq
trivy sbom  ./sbom2.json 
```

### Use Trivy for Kubernetes deployments 
```
kubectl run p1 --image=nginx
kubectl run p2 --image=httpd
kubectl run p3 --image=alpine -- sleep 1000
kubectl get pods -o=jsonpath='{range.items[*]}{"\n"}{.metadata.name}{":\t"}{range.spec.containers[*]}{.image}{","}{end}{end}' |sort
trivy image --severity HIGH,CRITICAL nginx
trivy image --severity HIGH,CRITICAL httpd
trivy image --severity HIGH,CRITICAL alpine
echo p1 $'\n'p2 > /tmp/badimages.txt
```
