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

application 'guestbook' created
```

Step3: Get the App Details

```shell
$ argocd app get guestbook
Name:               argocd/guestbook
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          guestbook
URL:                https://127.0.0.1:38739/applications/guestbook
Repo:               https://github.com/ramanNarasimhan77/kcd-gitops-workshop.git
Target:
Path:               apps/guestbook
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        OutOfSync from  (bc989e8)
Health Status:      Missing

GROUP  KIND        NAMESPACE  NAME          STATUS     HEALTH   HOOK  MESSAGE
       Service     guestbook  guestbook-ui  OutOfSync  Missing
apps   Deployment  guestbook  guestbook-ui  OutOfSync  Missing
```

Step4:  Sync the app 

```shell
$ argocd app sync guestbook
TIMESTAMP                  GROUP        KIND   NAMESPACE                  NAME    STATUS    HEALTH        HOOK  MESSAGE
2023-05-24T17:00:26+05:30            Service   guestbook          guestbook-ui  OutOfSync  Missing
2023-05-24T17:00:26+05:30   apps  Deployment   guestbook          guestbook-ui  OutOfSync  Missing
2023-05-24T17:00:26+05:30            Service   guestbook          guestbook-ui    Synced  Healthy
2023-05-24T17:00:26+05:30            Service   guestbook          guestbook-ui    Synced   Healthy              service/guestbook-ui created
2023-05-24T17:00:26+05:30   apps  Deployment   guestbook          guestbook-ui  OutOfSync  Missing              deployment.apps/guestbook-ui created
2023-05-24T17:00:26+05:30   apps  Deployment   guestbook          guestbook-ui    Synced  Progressing              deployment.apps/guestbook-ui created

Name:               argocd/guestbook
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          guestbook
URL:                https://127.0.0.1:38739/applications/guestbook
Repo:               https://github.com/ramanNarasimhan77/kcd-gitops-workshop.git
Target:
Path:               apps/guestbook
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (bc989e8)
Health Status:      Progressing

Operation:          Sync
Sync Revision:      bc989e85381547bce6b9f6ee93b528425d4f3e4a
Phase:              Succeeded
Start:              2023-05-24 17:00:26 +0530 IST
Finished:           2023-05-24 17:00:26 +0530 IST
Duration:           0s
Message:            successfully synced (all tasks run)

GROUP  KIND        NAMESPACE  NAME          STATUS  HEALTH       HOOK  MESSAGE
       Service     guestbook  guestbook-ui  Synced  Healthy            service/guestbook-ui created
apps   Deployment  guestbook  guestbook-ui  Synced  Progressing        deployment.apps/guestbook-ui created```

Step 5: Get app details again and verify it is synced

```shell
$ argocd app get guestbook
Name:               argocd/guestbook
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          guestbook
URL:                https://127.0.0.1:38739/applications/guestbook
Repo:               https://github.com/ramanNarasimhan77/kcd-gitops-workshop.git
Target:
Path:               apps/guestbook
SyncWindow:         Sync Allowed
Sync Policy:        <none>
Sync Status:        Synced to  (bc989e8)
Health Status:      Healthy

GROUP  KIND        NAMESPACE  NAME          STATUS  HEALTH   HOOK  MESSAGE
       Service     guestbook  guestbook-ui  Synced  Healthy        service/guestbook-ui created
apps   Deployment  guestbook  guestbook-ui  Synced  Healthy        deployment.apps/guestbook-ui created
```

```shell
$ kubectl get all -n guestbook
NAME                                READY   STATUS    RESTARTS   AGE
pod/guestbook-ui-754d46fbf6-6ckrd   1/1     Running   0          117s

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/guestbook-ui   ClusterIP   10.109.73.201   <none>        80/TCP    117s

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/guestbook-ui   1/1     1            1           117s

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/guestbook-ui-754d46fbf6   1         1         1       117s
```

Step6 : Delete the application

```shell
$ argocd app delete guestbook
Are you sure you want to delete 'guestbook' and all its resources? [y/n] y
application 'guestbook' deleted
```