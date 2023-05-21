# Accessing the UI

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
