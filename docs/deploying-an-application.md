# Deploying an application


## Using cli 

Step1: Verify you are connected to the right context

```shell
$ argocd context
CURRENT  NAME             SERVER
*        127.0.0.1:38739  127.0.0.1:38739
```

Step2: Deploy sample application using the CLI

```shell
$ argocd app create guestbook --repo https://github.com/ramanNarasimhan77/kcd-gitops-workshop.git --path apps/guestbook --dest-namespace guestbook --dest-server https://kubernetes.default.svc --sync-option CreateNamespace=true