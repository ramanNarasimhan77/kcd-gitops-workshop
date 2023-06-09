# Argo-CD installation

## Installation options

See [ArgoCD Installation docs](https://argo-cd.readthedocs.io/en/stable/operator-manual/installation/#installation)

Installation possible in different modes
* Multitenant - Maintained by a platform team where ArgoCD is used by different Developer teams.
  * Non HA
  * HA
  Single Namespace & Clusterwide installations are possible

* Core
  * When ArgoCD is used exclusively by cluster admins
  * no UI

Ways of installation:
* Manifests
* Kustomize
* [Community Helm chart](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd)
  * Redis-HA
  * HAProxy


### Standard HA install

Refer [ArgoCD Releases page](https://github.com/argoproj/argo-cd/releases/) for installing latest
ArgoCD release version in HA mode.

```shell 
$ kubectl create namespace argocd
namespace/argocd created

$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.7.2/manifests/ha/install.yaml
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/applicationsets.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io created
serviceaccount/argocd-application-controller created
serviceaccount/argocd-applicationset-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-notifications-controller created
serviceaccount/argocd-redis-ha created
serviceaccount/argocd-redis-ha-haproxy created
serviceaccount/argocd-repo-server created
serviceaccount/argocd-server created
role.rbac.authorization.k8s.io/argocd-application-controller created
role.rbac.authorization.k8s.io/argocd-applicationset-controller created
role.rbac.authorization.k8s.io/argocd-dex-server created
role.rbac.authorization.k8s.io/argocd-notifications-controller created
role.rbac.authorization.k8s.io/argocd-redis-ha created
role.rbac.authorization.k8s.io/argocd-redis-ha-haproxy created
role.rbac.authorization.k8s.io/argocd-server created
clusterrole.rbac.authorization.k8s.io/argocd-application-controller created
clusterrole.rbac.authorization.k8s.io/argocd-server created
rolebinding.rbac.authorization.k8s.io/argocd-application-controller created
rolebinding.rbac.authorization.k8s.io/argocd-applicationset-controller created
rolebinding.rbac.authorization.k8s.io/argocd-dex-server created
rolebinding.rbac.authorization.k8s.io/argocd-notifications-controller created
rolebinding.rbac.authorization.k8s.io/argocd-redis-ha created
rolebinding.rbac.authorization.k8s.io/argocd-redis-ha-haproxy created
rolebinding.rbac.authorization.k8s.io/argocd-server created
clusterrolebinding.rbac.authorization.k8s.io/argocd-application-controller created
clusterrolebinding.rbac.authorization.k8s.io/argocd-server created
configmap/argocd-cm created
configmap/argocd-cmd-params-cm created
configmap/argocd-gpg-keys-cm created
configmap/argocd-notifications-cm created
configmap/argocd-rbac-cm created
configmap/argocd-redis-ha-configmap created
configmap/argocd-redis-ha-health-configmap created
configmap/argocd-ssh-known-hosts-cm created
configmap/argocd-tls-certs-cm created
secret/argocd-notifications-secret created
secret/argocd-secret created
service/argocd-applicationset-controller created
service/argocd-dex-server created
service/argocd-metrics created
service/argocd-notifications-controller-metrics created
service/argocd-redis-ha created
service/argocd-redis-ha-announce-0 created
service/argocd-redis-ha-announce-1 created
service/argocd-redis-ha-announce-2 created
service/argocd-redis-ha-haproxy created
service/argocd-repo-server created
service/argocd-server created
service/argocd-server-metrics created
deployment.apps/argocd-applicationset-controller created
deployment.apps/argocd-dex-server created
deployment.apps/argocd-notifications-controller created
deployment.apps/argocd-redis-ha-haproxy created
deployment.apps/argocd-repo-server created
deployment.apps/argocd-server created
statefulset.apps/argocd-application-controller created
statefulset.apps/argocd-redis-ha-server created
networkpolicy.networking.k8s.io/argocd-application-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-applicationset-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-dex-server-network-policy created
networkpolicy.networking.k8s.io/argocd-notifications-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-redis-ha-proxy-network-policy created
networkpolicy.networking.k8s.io/argocd-redis-ha-server-network-policy created
networkpolicy.networking.k8s.io/argocd-repo-server-network-policy created
networkpolicy.networking.k8s.io/argocd-server-network-policy created
```

Verify that all services are running

```shell

$ kubectl get all -n argocd
NAME                                                   READY   STATUS    RESTARTS   AGE
pod/argocd-application-controller-0                    1/1     Running   0          12m
pod/argocd-applicationset-controller-86fc5c85-xt9g4    1/1     Running   0          12m
pod/argocd-dex-server-7f4689df5-vlhf2                  1/1     Running   0          12m
pod/argocd-notifications-controller-59f78959c8-nm6cc   1/1     Running   0          12m
pod/argocd-redis-ha-haproxy-7d7c895d48-bw86r           1/1     Running   0          12m
pod/argocd-redis-ha-haproxy-7d7c895d48-d7zwq           1/1     Running   0          12m
pod/argocd-redis-ha-haproxy-7d7c895d48-q8fmk           1/1     Running   0          12m
pod/argocd-redis-ha-server-0                           3/3     Running   0          12m
pod/argocd-redis-ha-server-1                           3/3     Running   0          9m26s
pod/argocd-redis-ha-server-2                           3/3     Running   0          8m25s
pod/argocd-repo-server-7f6b969896-6kcs9                1/1     Running   0          12m
pod/argocd-repo-server-7f6b969896-tvmxq                1/1     Running   0          12m
pod/argocd-server-59f64ffbbf-2r6gr                     1/1     Running   0          12m
pod/argocd-server-59f64ffbbf-bwfx9                     1/1     Running   0          12m

NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.102.217.12   <none>        7000/TCP,8080/TCP            12m
service/argocd-dex-server                         ClusterIP   10.99.75.8      <none>        5556/TCP,5557/TCP,5558/TCP   12m
service/argocd-metrics                            ClusterIP   10.98.9.172     <none>        8082/TCP                     12m
service/argocd-notifications-controller-metrics   ClusterIP   10.96.81.132    <none>        9001/TCP                     12m
service/argocd-redis-ha                           ClusterIP   None            <none>        6379/TCP,26379/TCP           12m
service/argocd-redis-ha-announce-0                ClusterIP   10.96.91.211    <none>        6379/TCP,26379/TCP           12m
service/argocd-redis-ha-announce-1                ClusterIP   10.97.231.19    <none>        6379/TCP,26379/TCP           12m
service/argocd-redis-ha-announce-2                ClusterIP   10.109.34.29    <none>        6379/TCP,26379/TCP           12m
service/argocd-redis-ha-haproxy                   ClusterIP   10.99.7.166     <none>        6379/TCP                     12m
service/argocd-repo-server                        ClusterIP   10.107.191.8    <none>        8081/TCP,8084/TCP            12m
service/argocd-server                             ClusterIP   10.107.242.55   <none>        80/TCP,443/TCP               12m
service/argocd-server-metrics                     ClusterIP   10.98.249.120   <none>        8083/TCP                     12m

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           12m
deployment.apps/argocd-dex-server                  1/1     1            1           12m
deployment.apps/argocd-notifications-controller    1/1     1            1           12m
deployment.apps/argocd-redis-ha-haproxy            3/3     3            3           12m
deployment.apps/argocd-repo-server                 2/2     2            2           12m
deployment.apps/argocd-server                      2/2     2            2           12m

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-86fc5c85    1         1         1       12m
replicaset.apps/argocd-dex-server-7f4689df5                  1         1         1       12m
replicaset.apps/argocd-notifications-controller-59f78959c8   1         1         1       12m
replicaset.apps/argocd-redis-ha-haproxy-7d7c895d48           3         3         3       12m
replicaset.apps/argocd-repo-server-7f6b969896                2         2         2       12m
replicaset.apps/argocd-server-59f64ffbbf                     2         2         2       12m

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     12m
statefulset.apps/argocd-redis-ha-server          3/3     12m

```

## Download CLI into local machine

```shell 
$ wget -O argocd https://github.com/argoproj/argo-cd/releases/download/v2.7.2/argocd-linux-amd64 
--2023-05-24 16:16:32--  https://github.com/argoproj/argo-cd/releases/download/v2.7.2/argocd-linux-amd64
Resolving github.com (github.com)... 20.207.73.82
Connecting to github.com (github.com)|20.207.73.82|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/120896210/936aafb5-2560-4808-b66d-523afc0cd48e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230524%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230524T104633Z&X-Amz-Expires=300&X-Amz-Signature=11a94aa0db8e11daf0ecf753af589d32870071eb847214673b8a406ed5576e78&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=120896210&response-content-disposition=attachment%3B%20filename%3Dargocd-linux-amd64&response-content-type=application%2Foctet-stream [following]
--2023-05-24 16:16:33--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/120896210/936aafb5-2560-4808-b66d-523afc0cd48e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230524%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230524T104633Z&X-Amz-Expires=300&X-Amz-Signature=11a94aa0db8e11daf0ecf753af589d32870071eb847214673b8a406ed5576e78&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=120896210&response-content-disposition=attachment%3B%20filename%3Dargocd-linux-amd64&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.110.133, 185.199.109.133, 185.199.111.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.110.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 140367676 (134M) [application/octet-stream]
Saving to: ‘argocd’

argocd                                          100%[====================================================================================================>] 133.86M  34.4MB/s    in 4.1s

$ chmod +x argocd

$ sudo mv argocd /usr/local/bin/

```

Verify that argocd client is installed and available 

```shell
$ argocd version --client
argocd: v2.7.2+cbee7e6
  BuildDate: 2023-05-12T14:06:49Z
  GitCommit: cbee7e6011407ed2d1066c482db74e97e0cc6bdb
  GitTreeState: clean
  GoVersion: go1.19.9
  Compiler: gc
  Platform: linux/amd64 
```