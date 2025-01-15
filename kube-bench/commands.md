curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.9.0/kube-bench_0.9.0_linux_amd64.deb -o kube-bench_0.9.0_linux_amd64.deb

sudo apt install ./kube-bench_0.9.0_linux_amd64.deb -f

kube-bench

docker run --pid=host -v /etc:/etc:ro -v /var:/var:ro -v $(which kubectl):/usr/local/mount-from-host/bin/kubectl -v ~/.kube:/.kube -e KUBECONFIG=/.kube/config -t aquasec/kube-bench:latest run --targets=master

docker run --pid=host -v /etc:/etc:ro -v /var:/var:ro -v $(which kubectl):/usr/local/mount-from-host/bin/kubectl -v ~/.kube:/.kube -e KUBECONFIG=/.kube/config -t aquasec/kube-bench:latest run --targets=node
