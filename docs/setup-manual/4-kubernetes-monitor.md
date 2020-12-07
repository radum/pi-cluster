### (4/7) Deploy Prometheus and Grafana to monitor a Kubernetes cluster

[Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/) is a common combination of tools to build up a monitoring system where Prometheus acts as a data collector pulling periodically metrics from different systems and Grafana as a dashboard solution to visualise the data.

[cluster-monitoring](https://github.com/carlosedp/cluster-monitoring) offers a scripts to automate the installation and configuration of Prometheus + Grafana and provides a lot of dashboards for Kubernetes out of the box.

**1. Prerequisite**

In order to run entirely the tutorial, we will need:

* A running Kubernetes cluster (see previous articles if you haven't set this up yet)
* Install Golang on the machine that runs kubectl

```
<!-- Or use brew, or the provided instal script. -->
$ sudo apt install golang-go
$ export PATH=$PATH:$(go env GOPATH)/bin
$ export GOPATH=$(go env GOPATH)
```

**2. Configuration**

```bash
git clone https://github.com/carlosedp/cluster-monitoring.git
```

Open the file `cluster-monitoring/vars.jsonnet` and modify the following sections:

Enable k3s and put the IP of our master node kube-master.

```
k3s: {
    enabled: true,
    master_ip: ['192.168.0.100'],
  },
```

The suffix domain is used to deploy an ingress to access Prometheus `prometheus.<suffixDomain>` and Grafana `grafana.<suffixDomain>`. You can manually configure a DNS entry to point to `192.168.0.240` (Load Balancer IP of the Nginx) or used [nip.io](https://nip.io/) to automatically resolve a domain to an IP (basically it resolves `<anything>.<ip>.nip.io` by `<ip>` without requiring any other configuration).

```
suffixDomain: '192.168.0.240.nip.io',
```

Enable the persistence to store the metrics (Prometheus) and dashboard settings (Grafana).

```
enablePersistence: {
    prometheus: true,
    grafana: true,
  },
```

Enable additional resources (armExporter will bring temperature monitoring also for PI).

```
{
	name: 'armExporter',
	enabled: true,
	file: import 'modules/arm_exporter.jsonnet',
},
{
	name: 'metallbExporter',
	enabled: true,
	file: import 'modules/metallb.jsonnet',
}
```

**3. Installation**

```bash
cd cluster-monitoring/

# First run make vendor to download all the necessary packages.
make vendor

# Finally deploy the monitoring stack with the command make deploy.
make deploy
```

**Repeat this command again if you see some errors as a result.**

Once deployed, check that the namespace `monitoring` has all the components up and running.

```bash
kubectl get pods -n monitoring -o wide

# NAME                                   READY   STATUS    RESTARTS   AGE     IP             NODE          NOMINATED NODE   READINESS GATES
# prometheus-operator-56d66c5dc6-8ljsb   1/1     Running   0          7m54s   10.42.0.154    kube-master   <none>           <none>
# alertmanager-main-0                    2/2     Running   0          7m43s   10.42.0.155    kube-master   <none>           <none>
# kube-state-metrics-5c6fb7544b-6p956    3/3     Running   0          7m36s   10.42.0.157    kube-master   <none>           <none>
# node-exporter-h48d6                    2/2     Running   0          7m34s   192.168.0.22   kube-master   <none>           <none>
# prometheus-adapter-679db899b7-x89l6    1/1     Running   0          7m22s   10.42.0.158    kube-master   <none>           <none>
# grafana-6fb45f9c8-j8qbv                1/1     Running   0          7m41s   10.42.0.159    kube-master   <none>           <none>
# prometheus-k8s-0                       3/3     Running   1          7m15s   10.42.0.161    kube-master   <none>           <none>
```

**4. Dashboard**

After configuring and deploying our Kubernetes Monitoring stack, you can now access the different components:

* Prometheus: https://prometheus.192.168.0.240.nip.io
* Grafana: https://grafana.192.168.0.240.nip.io

Click on the Grafana link and login with the default login/password `admin/admin` (you will be asked to choose a new password).

You can now finally enjoy a lot of pre-configured dashboards for your Kubernetes cluster.
