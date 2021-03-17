# Personal Pi Cluster

At the moment its main purpose is to run a media center and some dev tools.

The follwing guide assumes everything will be done manually. Hopefully in the future this could be provisioned using Ansible or at least a script.

## Requirements

Add **K8S** (former billimek) and **bananaspliff** helm charts repos. We will use some helm charts from both.

```bash
helm repo add k8s-at-home https://k8s-at-home.com/charts/
helm repo add bananaspliff https://bananaspliff.github.io/geek-charts
helm repo update
```

## Architecture

- TODO

## Setup Steps

### [(1/9) Pi setup](1-pi-setup.md)

### [(2/9) Install and configure a Kubernetes cluster with k3s](2-install-k3s.md)

### [(3/9) Self-host your Media Center On Kubernetes with Plex, Sonarr, Radarr, Transmission and Jackett](3-media-center.md)

### [(4/9) Deploy Prometheus and Grafana to monitor a Kubernetes cluster](4-kubernetes-monitor.md)

### (5/9) Heimdall Application Dashboard (TODO: CONFIG PVC BUG)

[Heimdall](https://heimdall.site/) is a application dashboard.

**TODO: Using a PVC for config fails for some reason**

**1. Create the Helm config file media.heimdall.values.yml**

```
# services.heimdall.values.yml
# Content: [cluster/base/services/heimdall/services.heimdall.values.yml]
```

**2. Install the chart billimek/heimdall**

Execute the following command to install the chart `billimek/heimdall` with the above configuration onto the namespace `services`.

```bash
# If not already done
kubectl create namespace services

helm install heimdall billimek/heimdall \
    --values cluster/base/services/heimdall/services.heimdall.values.yml \
    --namespace services
```

**3. Open heimdall**

http://heimdall.192.168.0.240.nip.io/

**2. Access the UI and configure**

Access http://media.192.168.0.242.nip.io/ and configure (user will be admin).

### (6/9) Calibre WEB **(TODO: WIP)**

```bash
helm install calibre-web k8s-at-home/calibre-web \
    --values cluster/base/services/calibre-web/services.calibre-web.values.yml \
    --namespace media

# There is no ARM image available so this can't be used.
helm install calibre k8s-at-home/calibre \
    --values cluster/base/services/calibre/services.calibre.values.yml \
    --namespace media
```

http://calibre.192.168.0.240.nip.io/

/books/calibre/metadata.db

### (7/9) Lazylibrarian **(TODO: Transmission doesn't work OK)**

[LazyLibrarian](LazyLibrarian) LazyLibrarian is a program to follow authors and grab metadata for all your digital reading needs.

Using the helm chart from here: https://github.com/aldoborrero/k8s-usenet/tree/master/lazylibrarian

**1. Create NFS folder structure**

```
/mnt/ssd/services/lazylibrarian/config
/mnt/ssd/services/lazylibrarian/books
/mnt/ssd/services/lazylibrarian/downloads
```

**2. Add a config.ini in the config folder**

> NOTE: Not needed as it can be set from the Config tab.

Thie solves the LibGen premission denied issues.

```
[General]
user_agent = Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
download_dir = /downloads
ebook_dir = /books
```

**3. Run the helm chart**

```bash
kubectl apply -f cluster/base/services/lazylibrarian/lazylibrarian.pvc.yml

# helm install lazylibrarian k8s-usenet/lazylibrarian --values cluster/base/services/lazylibrarian/services.lazylibrarian.values.yml --namespace services

helm install lazylibrarian cluster/base/services/lazylibrarian/helm --values cluster/base/services/lazylibrarian/services.lazylibrarian.values.yml --namespace services
```

**4. Configure providers and downloaders**

Providers:

* http://gen.lib.rus.ec

Downloaders:

* transmission-transmission-openvpn.media:80 (/media/downloads/transmission TODO: Add a new share path to put them in librarian PVC)

**5. Access Lazylibrarian Dashboard**

http://lazylibrarian.192.168.0.240.nip.io/home

### (8/9) Flood - monitoring service for various torrent clients **(TODO: WIP)**

```bash
helm install flood k8s-at-home/flood \
    --values cluster/base/services/flood/services.flood.values.yml \
    --namespace media
```

### (8/9) YoutubeDL-Material - a Material Design frontend for youtube-dl **(TODO: WIP)**

```bash
helm install youtubedl k8s-at-home/youtubedl-material \
    --values cluster/base/services/youtubedl-material/services.youtubedl-material.values.yml \
    --namespace media
```

### (8/9) OMBI - self-hosted web application that automatically gives your shared Plex or Emby users the ability to request content by themselves **(TODO: WIP)**

```bash
helm install ombi k8s-at-home/ombi \
    --values cluster/base/media/ombi/media.ombi.values.yml \
    --namespace media
```

### (9/9) Lidarr - Sonarr for music

```
# config.xml
<Config>
  <UrlBase>/lidarr</UrlBase>
</Config>
```

```bash
helm install lidarr k8s-at-home/lidarr \
    --values cluster/base/media/lidarr/media.lidarr.values.yml \
    --namespace media
```
