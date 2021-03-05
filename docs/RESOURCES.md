# Resources

Various links and resources around the web on how to build a Pi Cluster.

## Implementation Resources

### Tutorials

-  Build your very own self-hosting platform with Raspberry Pi and Kubernetes https://kauri.io/build-your-very-own-self-hosting-platform-with-raspberry-pi-and-kubernetes-introduction/1229f21044ef4bff8df35875d6803776/a
- Step by Step slow guide — Kubernetes Cluster on Raspberry Pi 4B https://levelup.gitconnected.com/step-by-step-slow-guide-kubernetes-cluster-on-raspberry-pi-4b-part-1-6e4179c89cbc
- Raspberry Pi Cluster https://www.jeffgeerling.com/blog/2020/raspberry-pi-cluster-episode-1-introduction-clusters

### Articles

- https://www.raspberrypi.org/blog/five-years-of-raspberry-pi-clusters/
- https://medium.com/@alexellisuk/five-years-of-raspberry-pi-clusters-77e56e547875
- https://mtlynch.io/digitizing-home-videos-walkthrough/
- https://blog.dsb.dev/posts/accessing-my-k3s-cluster-from-anywhere-with-tailscale/index.html

### Cluster Resources

#### Helm repos

- Billimek charts https://github.com/billimek/billimek-charts/ [DEPRECATED]
- Geek Helm charts for seedbox https://github.com/bananaspliff/geek-charts
- k8s@Home collection of helm charts https://github.com/k8s-at-home/charts

#### K3s Implementations

- k3s cluster backed by Flux (GitOps) up and running on a cluster of RPi4 https://github.com/onedr0p/k3s-gitops-arm
- Mediacenter for Raspberry Pi K3s cluster https://github.com/olafkfreund/mediacenter

- from Zero to KUBECONFIG in < 1 min 🚀 https://github.com/alexellis/k3sup
- Kubernetes apps, the easy way 😎 https://github.com/alexellis/arkade

#### Docker Implementations

- My Docker Compose for my Udoo Bolt Home Media Server https://github.com/jarrad-roam/bolt-media
- Personal version of smarthomebeginner docker home media server https://github.com/thoroc/home-server
- A collection of scripts and dockers for my home media server https://github.com/danshilm/Tatooine-380
- Docker compose home media automation server https://github.com/sdoDevelop/homemedia
- Docker-compose for home media server setup https://github.com/Snuffsis/mediaserver-compose
- My Home Media Server in docker compose https://github.com/Noctris/mediaserver-docker
- Raspberry Pi Media Server using Hypriot OS https://github.com/austinmcconnell/raspberry-media-server
- This is a collection of docker-compose files used to build a media server https://github.com/Maskime/media-server

#### Other Implementations

- Ansible playbook for my Home Media Server https://github.com/guillaumebriday/homelab-docker-ansible

- https://github.com/onedr0p/home-cluster
- https://github.com/onedr0p/home-operations

- https://github.com/billimek/homelab-infrastructure
- https://github.com/billimek/k8s-gitops
- https://github.com/PixelJonas/homecluster
- https://github.com/PixelJonas/homeserver

- https://github.com/bjw-s/k8s-gitops

#### PI Bootstrap examples

- Ansible playbook to provision a cluster of Raspberry Pi's with k3s https://github.com/olafkfreund/ansible-k3s-rpi
- Raspbernetes - Kubernetes Cluster https://github.com/raspbernetes/k8s-gitops

## PI Compatible Resources

### PI Compatible Contaiers

- Influx ARM https://hub.docker.com/r/arm32v7/influxdb/tags

### PI Compatible Compatible OS

- Rasbian https://www.raspberrypi.org/downloads/raspbian/
- Ubuntu https://ubuntu.com/download/raspberry-pi

## Monitoring Resources

### Monitoring Tools

- Cluster monitoring stack for clusters based on Prometheus Operator https://github.com/carlosedp/cluster-monitoring

### Grafana dashboards

- Raspberry Pi Monitoring https://grafana.com/grafana/dashboards/10578
- AlexNAS Dashboard https://grafana.com/grafana/dashboards/12090 (demo https://camo.githubusercontent.com/eb9dd6e0b4a7cd73f5c5710bd59813c9ecaedb00/68747470733a2f2f692e726564642e69742f6e6d7070686938616c657434312e706e67)

## Community Resources

- https://www.reddit.com/r/picluster/new/

## Useful commands

**1. Helm upgrade**

```bash
helm upgrade { same props as for install }
```

**2. Helm uninstall**

```bash
helm uninstall {name} -n {namespace}
```
