# k3sup

> All these commands are run from your computer, not the RPi.

```bash
# Install k3sup locally
cd ~/Downloads
curl -sLS https://get.k3sup.dev | sh
# These should not be necesarry if the above one works OK
# sudo cp k3sup /usr/local/bin/
# k3sup --help

# Install k3s on master node
k3sup install --ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster \
	--k3s-extra-args '--disable servicelb'
    # --k3s-extra-args '--disable servicelb --disable traefik'

# Optional argument
# --ssh-key ~/.ssh/pi-cluster
# --disable metrics-server

# If I setup a USB drive for data this can be used also
# --default-local-storage-path /k3s-local-storage

# Make kubeconfig accessable globally
mkdir ~/.kube
mv ./kubeconfig ~/.kube/config

# Join worker nodes into the cluster
k3sup join --ip 192.168.0.101 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.102 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.103 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

# You should be able to see all your nodes
kubectl get nodes

# View config for the Cluster
kubectl get configmap coredns -n kube-system -o yaml

# You can see the IP address of the LoadBalancer created with the following command:
kubectl -n kube-system get svc
```

## Upgrading

> All these commands are run from your computer, not the RPi.

To upgrade K3s on each node you just need to run the install again. https://github.com/alexellis/k3sup/issues/161

To get the latest version of K3s https://github.com/rancher/k3s/tags

To upgrade k3sup locally outside the PI run `curl -sLS https://get.k3sup.dev | sh`

Latest k3sup commit run https://github.com/alexellis/k3sup/commit/9717ee3b75a0c2c4eb515ecd0fbad3553d53a1ff

```bash
# Install k3s on master node
k3sup install --ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster \
	--k3s-extra-args '--disable servicelb'
	# --k3s-extra-args '--disable servicelb --disable traefik'

# Update worker nodes into the cluster
k3sup join --ip 192.168.0.101 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.102 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.103 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.25.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster
```

After running the command example output

```
Running: k3sup install
2022/03/09 22:01:20 192.168.0.100
Public IP: 192.168.0.100
[INFO]  Using v1.25.4+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.25.4+k3s1/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.25.4+k3s1/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Skipping installation of SELinux RPM
[INFO]  Skipping /usr/local/bin/kubectl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/crictl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, already exists
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
Result: [INFO]  Using v1.25.4+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.25.4+k3s1/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.25.4+k3s1/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Skipping installation of SELinux RPM
[INFO]  Skipping /usr/local/bin/kubectl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/crictl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, already exists
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
[INFO]  systemd: Starting k3s
 Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.

Saving file to: /Users/redacted/kubeconfig

# Test your cluster with:
export KUBECONFIG=/Users/redacted/kubeconfig
kubectl config set-context default
kubectl get node -o wide
```

If it fails there must be a zombie process running so restart the cluster.

You can check the logs with:

```
sudo journalctl -r -u k3s
```
