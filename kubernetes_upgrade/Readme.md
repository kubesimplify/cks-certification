```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.32.2-*' && \
sudo apt-mark hold kubeadm
sudo kubeadm upgrade apply v1.32.2
kubectl drain cplane-01 --ignore-daemonsets
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.32.2-*' kubectl='1.32.2-*' && \
sudo apt-mark hold kubelet kubectl
sudo systemctl daemon-reload
sudo systemctl restart kubelet
kubectl uncordon cplane-01
```
For Node

```
sudo kubeadm upgrade node
```
