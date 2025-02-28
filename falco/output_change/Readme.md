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
### Run shell inside a container 
```
docker run --name ubuntu bash --rm -i -t ubuntu bash
```
### Change Falco Rules 
`output: "[%evt.time][%container.id] [%container.name]"`