# Exploring Application sets


## Create Application using List Generator

```shell
$ kubectl apply -f applicationSets/list-generator.yaml
applicationset.argoproj.io/applicationset-list-generator created

$ kubectl get applicationset -n argocd
NAME                            AGE
applicationset-list-generator   94s

$ kubectl get application -n argocd
NAME                 SYNC STATUS   HEALTH STATUS
my-guestbook         Synced        Healthy
sealed-secrets-app   Synced        Healthy
sync-waves-app       Synced        Healthy
```

## Create Applications using Git Generator

```shell
$  kubectl apply -f applicationSets/git-generator.yaml
applicationset.argoproj.io/applicationset-git-generator configured
```

```shell
$ kubectl get appset -n argocd
NAME                           AGE
applicationset-git-generator   109s

$ kubectl get applications -n argocd
NAME             SYNC STATUS   HEALTH STATUS
guestbook        Synced        Healthy
sealed-secrets   Synced        Healthy
```