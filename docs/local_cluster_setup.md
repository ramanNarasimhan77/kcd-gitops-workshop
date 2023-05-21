# Setting up a local cluster using Minikube

## Download Minikube
Refer https://minikube.sigs.k8s.io/docs/start/ to download minikube for your operating system

## Start a local cluster

I am simulating a real cluster by creating a multinode Minikube cluster.

```shell
$ minikube start --kubernetes-version=v1.26.3 --nodes=3
😄  minikube v1.30.1 on Ubuntu 22.04 (amd64)
✨  Automatically selected the docker driver. Other choices: podman, none, ssh
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass

👍  Starting worker node minikube-m02 in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.49.2
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ env NO_PROXY=192.168.49.2
🔎  Verifying Kubernetes components...

👍  Starting worker node minikube-m03 in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.49.2,192.168.49.3
🐳  Preparing Kubernetes v1.26.3 on Docker 23.0.2 ...
    ▪ env NO_PROXY=192.168.49.2
    ▪ env NO_PROXY=192.168.49.2,192.168.49.3
🔎  Verifying Kubernetes components...
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

### Check status

```shell
$ kubectl get nodes -o wide
NAME           STATUS   ROLES           AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION                      CONTAINER-RUNTIME
minikube       Ready    control-plane   3m48s   v1.26.3   192.168.49.2   <none>        Ubuntu 20.04.5 LTS   5.15.90.1-microsoft-standard-WSL2   docker://23.0.2
minikube-m02   Ready    <none>          3m9s    v1.26.3   192.168.49.3   <none>        Ubuntu 20.04.5 LTS   5.15.90.1-microsoft-standard-WSL2   docker://23.0.2
minikube-m03   Ready    <none>          2m48s   v1.26.3   192.168.49.4   <none>        Ubuntu 20.04.5 LTS   5.15.90.1-microsoft-standard-WSL2   docker://23.0.2

$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

minikube-m02
type: Worker
host: Running
kubelet: Running

minikube-m03
type: Worker
host: Running
kubelet: Running
```

### Add additional addons and configure them

1. [CSI Hostpath Driver](https://minikube.sigs.k8s.io/docs/tutorials/volume_snapshots_and_csi/)  

Necessary because [Default hostpath provisioner](https://minikube.sigs.k8s.io/docs/handbook/persistent_volumes/) does not work on multinode minikube setup.

```shell
$ minikube addons enable volumesnapshots

💡  volumesnapshots is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image registry.k8s.io/sig-storage/snapshot-controller:v6.1.0
🌟  The 'volumesnapshots' addon is enabled

$ minikube addons enable csi-hostpath-driver
💡  csi-hostpath-driver is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image registry.k8s.io/sig-storage/csi-snapshotter:v6.1.0
    ▪ Using image registry.k8s.io/sig-storage/csi-provisioner:v3.3.0
    ▪ Using image registry.k8s.io/sig-storage/csi-attacher:v4.0.0
    ▪ Using image registry.k8s.io/sig-storage/csi-external-health-monitor-controller:v0.7.0
    ▪ Using image registry.k8s.io/sig-storage/csi-node-driver-registrar:v2.6.0
    ▪ Using image registry.k8s.io/sig-storage/hostpathplugin:v1.9.0
    ▪ Using image registry.k8s.io/sig-storage/livenessprobe:v2.8.0
    ▪ Using image registry.k8s.io/sig-storage/csi-resizer:v1.6.0
🔎  Verifying csi-hostpath-driver addon...
🌟  The 'csi-hostpath-driver' addon is enabled
```

2. Metrics server - Necessary if you wish to use HPA

```shell
$  minikube addons enable metrics-server
💡  metrics-server is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image registry.k8s.io/metrics-server/metrics-server:v0.6.3
🌟  The 'metrics-server' addon is enabled
```


