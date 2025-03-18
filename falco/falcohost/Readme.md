## Install Falco 
```
curl -L -O https://download.falco.org/packages/bin/x86_64/falco-0.40.0-x86_64.tar.gz
tar -xvf falco-0.40.0-x86_64.tar.gz
cp -R falco-0.40.0-x86_64/* /
apt update -y
apt install -y dkms make linux-headers-$(uname -r)
# If you use falcoctl driver loader to build the eBPF probe locally you need also clang toolchain
apt install -y clang llvm

```
## Falco command
```
sudo falco -M 500 -r /etc/falco/falco_rules.local.yaml
```