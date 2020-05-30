# Personal Pi Cluster

At the moment its main purpose is to run a media center and some dev tools.

This repo uses most of the work done by [onedr0p](https://github.com/onedr0p/) in his [version](https://github.com/onedr0p/k3s-gitops-arm), so kudos to him, thank you.

> **Note:** A lot of files in this project have **@CHANGEME** comments, these are things that are specific to my set up that you may need to change.

## Prerequisites

### Hardware

- 3x Raspberry Pi 4 (recommended 4GB RAM model)
- 3x SD cards (recommended 32GB)
- 3x USB 3.x flash drives (optional, recommended for local storage)
- A NFS server for storing persistent data (recommended for shared storage)

### Software

- [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [hypriot/flash](https://github.com/hypriot/flash)
- [alexellis/k3sup](https://github.com/alexellis/k3sup)

* * *

## Directory topology

```bash
.
├── ./ansible        # Ansible playbook to run after the RPis have been flashed
├── ./cluster        # Cluster templates, Heml files and so on
├── ./setup          # Setup of the cluster machines
├── ./docs           # Documentation
└── ./assets         # Documentation related files (mostly images)
```

* * *

## Network topology

> **Note:** The IPs bellow are specific to my current network.

|IP|Function|
|---|---|
|192.168.0.1|Router|
|192.168.0.8|NFS Server|
|192.168.0.100|k3s master (kube-master)|
|192.168.0.101|k3s worker (kube-worker-1)|
|192.168.0.102|k3s worker (kube-worker-2)|

Ideally we should build a VLAN for our IOT devices. Like this:

|IP|Function|
|---|---|
|192.168.0.1|Router (USG)|
|192.168.0.170|NFS Server|
|192.168.10.1/24|k3s cluster CIDR, VLAN 10|
|192.168.10.23|k3s master (kube-master)|
|192.168.10.24|k3s worker (kube-worker-1)|
|192.168.10.25|k3s worker (kube-worker-2)|

* * *

## Let's get started

### 1. Flash SD Card with Ubuntu

> See [ubuntu.md](docs/setup-ansible/ubuntu.md)

### 2. Provision RPis with Ansible

[Ansible](https://www.ansible.com) is a great automation tool and here I am using it to provision the RPis.

> See [ansible.md](docs/setup-ansible/ansible.md) and review the files in the [ansible](ansible) folder.

### 3. Install k3s on your RPis using k3sup

[k3sup](https://k3sup.dev) is a neat tool provided by [@alexellis](https://github.com/alexellis) that helps get your k3s cluster up and running quick.

> For manual deployment see [k3sup.md](docs/setup-ansible/k3sup.md), and for an automated script see [bootstrap-cluster.sh](setup/bootstrap-cluster.sh)
