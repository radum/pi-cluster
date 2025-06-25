# Proxmox on my old mac

## How to install

### Good to know

- Proxmox requires a wired network (not wifi)

### Videos

- https://www.youtube.com/watch?v=FsPYgZYXyZw


### Articles

- https://davenewman.tech/blog/install-proxmox-on-a-laptop/


## Setup

- [Passing Storage to a Container Getting Started with Proxmox 8](https://youtu.be/qa2Q7tZVol8)

### Docker / Portainer

```bash
# Install
sudo docker volume create portainer_data

sudo docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

# Update
sudo docker stop portainer
sudo docker rm portainer
sudo docker pull portainer/portainer-ce:latest
sudo docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

## Setup examples

- Proxmox VE Helper-Scripts - https://github.com/tteck/Proxmox/tree/main https://tteck.github.io/Proxmox/
- Ansible-based solution for rapidly deploying a Docker containerized cloud media server. - https://github.com/Cloudbox/Cloudbox

## How To's

### Resize a disk

After you use the UI you need to tell the OS it needs to use all the space

- https://forum.proxmox.com/threads/resize-disk-of-ubuntu-19-10-vm-on-proxmox.70352/
- https://unix.stackexchange.com/questions/138090/cant-resize-a-partition-using-resize2fs

## Links

- https://192.168.0.248:8006/
- https://192.168.0.240:9443/
