# Accessing ArgoCD

## Get the admin password

```shell
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d;echo
22bXx9gv-N6uoCWM
```

## Accessing ArgoCD via port-forward

In a dedicated terminal run

```shell
❯ minikube service argocd-server -n argocd
|-----------|---------------|-------------|--------------|
| NAMESPACE |     NAME      | TARGET PORT |     URL      |
|-----------|---------------|-------------|--------------|
| argocd    | argocd-server |             | No node port |
|-----------|---------------|-------------|--------------|
😿  service argocd/argocd-server has no node port
🏃  Starting tunnel for service argocd-server.
|-----------|---------------|-------------|------------------------|
| NAMESPACE |     NAME      | TARGET PORT |          URL           |
|-----------|---------------|-------------|------------------------|
| argocd    | argocd-server |             | http://127.0.0.1:35311 |
|           |               |             | http://127.0.0.1:36099 |
|-----------|---------------|-------------|------------------------|
[argocd argocd-server  http://127.0.0.1:35311
http://127.0.0.1:36099]
❗  Because you are using a Docker driver on linux, the terminal needs to be open to run it.
```

## Open the UI

Click on any one of the listed URLs to open the UI. Enter the Username as `admin` and the admin password retrieved earlier.


## Accessing using the CLI

```shell
$ argocd login 127.0.0.1:38739 --username admin  --insecure
Password:
'admin:login' logged in successfully
Context '127.0.0.1:38739' updated
```

```shell
$ argocd version
argocd: v2.7.2+cbee7e6
  BuildDate: 2023-05-12T14:06:49Z
  GitCommit: cbee7e6011407ed2d1066c482db74e97e0cc6bdb
  GitTreeState: clean
  GoVersion: go1.19.9
  Compiler: gc
  Platform: linux/amd64
argocd-server: v2.7.2+cbee7e6.dirty
  BuildDate: 2023-05-12T13:43:26Z
  GitCommit: cbee7e6011407ed2d1066c482db74e97e0cc6bdb
  GitTreeState: dirty
  GoVersion: go1.19.6
  Compiler: gc
  Platform: linux/amd64
  Kustomize Version: v5.0.1 2023-03-14T01:32:48Z
  Helm Version: v3.11.2+g912ebc1
  Kubectl Version: v0.24.2
  Jsonnet Version: v0.19.1
```