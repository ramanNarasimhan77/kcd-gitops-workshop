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

### 

### Standard HA install

Below command is taken from [Argocd Releases page](https://github.com/argoproj/argo-cd/releases/) for installing latest
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
networkpolicy.networking.k8s.io/argocd-server-network-policy created```
