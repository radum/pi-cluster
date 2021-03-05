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
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster \
    --k3s-extra-args '--no-deploy servicelb --no-deploy traefik'

# Optional argument
# --ssh-key ~/.ssh/pi-cluster
# --no-deploy metrics-server

# If I setup a USB drive for data this can be used also
# --default-local-storage-path /k3s-local-storage

# Make kubeconfig accessable globally
mkdir ~/.kube
mv ./kubeconfig ~/.kube/config

# Join worker nodes into the cluster
k3sup join --ip 192.168.0.101 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.102 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.103 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

# You should be able to see all your nodes
kubectl get nodes

# View config for the Cluster
kubectl get configmap coredns -n kube-system -o yaml
```

## Upgrading

> All these commands are run from your computer, not the RPi.

To upgrade K3s on each node you just need to run the install again. https://github.com/alexellis/k3sup/issues/161

To get the latest version of K3s https://github.com/rancher/k3s/tags

To upgrade k3sup locally outside the PI run `curl -sLS https://get.k3sup.dev | sh`

Latest k3sup commit run https://github.com/alexellis/k3sup/commit/29b43c8

```bash
# Install k3s on master node
k3sup install --ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster \
    --k3s-extra-args '--no-deploy servicelb --no-deploy traefik'

# Update worker nodes into the cluster
k3sup join --ip 192.168.0.101 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.102 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster

k3sup join --ip 192.168.0.103 \
    --server-ip 192.168.0.100 \
    --k3s-version v1.20.4+k3s1 \
    --user pi \
    --ssh-key ~/.ssh/pi-cluster
```
