Helm is package manager for kuberentes,which allows to kubernetes controllers or 3rd party applications update versions uninstall etc..

We can bundle applications can be bundle as helm charts

In Kuberentes we can install applications like nginx by running

```helm install my-nginx bitnami/nginx```

Helm charts are packages containing all the necessary resources and configurations to deploy an application or service on a Kubernetes cluster

Why should we use helm ?

1. Package management - Helm manages helm charts (YAML files bundled together) in Kubernetes

2. Version Control - ```helm install my-nginx bitnami/nginx --version 13.0.0```

3. Configuration Management - We can override configuration values using a YAML file

4. Managing Dependencies - Helm handles depenedencies between kuberenetes components ensuring a database is ready before applying web app

Why Helm is needed ?

Kuberenetes is a complex systemt with many moving parts (pods,deplployments,services etc ..).Helm simplifies the process by pacakaging everything into resusable charts. It's like having a one-click install for your entire application stack instad instead of manually cerating each component

How to install helm ?

https://helm.sh/docs/intro/install/

Note:- From version 3 no need to install anything on kubernetes cluster


Just like kubectl looks for current context of kubectl file and connect to respective kubernetes cluster Helm also uses context

```
 Deepak S   deepaks    kind get clusters                                                                              in pwsh at 18:00:46
learn-helm
 Deepak S   deepaks    helm version                                                                                   in pwsh at 18:00:49
version.BuildInfo{Version:"v3.18.6", GitCommit:"b76a950f6835474e0906b96c9ec68a2eff3a6430", GitTreeState:"clean", GoVersion:"go1.24.6"}
 Deepak S   deepaks    kubectl config current-context   
kind-learn-helm
```

Install Nginx in default namespace

1. Add the repository

3. Install nginx from the repsository

A helm repositoy consists of all the charts (Chart is a bundle package of the application Eg: grafana chart)

Common repositoy bitnami popular chart

```
 Deepak S   deepaks    helm repo add bitnami https://charts.bitnami.com/bitnami                                       in pwsh at 18:04:21
"bitnami" has been added to your repositories
 Deepak S   deepaks     


 Deepak S   deepaks     helm search repo bitnami | grep nginx                                                         in pwsh at 18:20:09
bitnami/nginx                                   21.1.23         1.29.1          NGINX Open Source is a web server that can be a...
bitnami/nginx-ingress-controller                12.0.7          1.13.1          NGINX Ingress Controller is an Ingress controll...
bitnami/nginx-intel                             2.1.15          0.4.9           DEPRECATED NGINX Open Source for Intel is a lig...
 Deepak S   deepaks    

```
We can also create repository and keep our chart for our microservices (E commerce - payment,shipments etc..)

```
 Deepak S   deepaks     helm install nginxv1 bitnami/nginx                                                            in pwsh at 18:20:14
NAME: nginxv1
LAST DEPLOYED: Fri Aug 29 18:35:04 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 21.1.23
APP VERSION: 1.29.1

 Deepak S   deepaks    kubectl get pods                                                                               in pwsh at 18:36:18
NAME                      READY   STATUS    RESTARTS   AGE
nginxv1-88b888469-8czsr   1/1     Running   0          83s
 Deepak S   deepaks      

```

What is release ?
Release is the deployed instance of the chart
Name of the release Eg: nginxv1  

how to install other helm charts which are not part of bitnami ?
Check official documentation, for example for AWS ingress loadbalancer

```
 Deepak S   deepaks    helm repo add eks https://aws.github.io/eks-charts                                             in pwsh at 20:27:01
"eks" has been added to your repositories
 Deepak S   deepaks    helm repo update eks                                                                           in pwsh at 20:29:15
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "eks" chart repository
Update Complete. ⎈Happy Helming!⎈
 Deepak S   deepaks      

 Deepak S   deepaks     helm search repo eks | grep load                                                              in pwsh at 20:38:51
eks/aws-load-balancer-controller                1.13.4          v2.13.4                                 AWS Load Balancer Controller Helm chart for Kub...
 Deepak S   deepaks       
Error: INSTALLATION FAILED: execution error at (aws-load-balancer-controller/templates/deployment.yaml:67:28): Chart cannot be installed without a valid clusterName!
 Deepak S   deepaks   
```
We are not passing arguements hence aws alb is not installing with helm

How to uninstall helm chart

```
 Deepak S   deepaks    helm list                                                                                      in pwsh at 20:40:06
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
nginxv1         default         1               2025-08-29 18:35:04.5396634 +0530 IST   deployed        nginx-21.1.23           1.29.1
prometheus      default         1               2025-08-29 18:41:58.2482426 +0530 IST   deployed        prometheus-2.1.23       3.5.0
 Deepak S   deepaks             


 Deepak S   deepaks    helm uninstall nginxv1                                                                         in pwsh at 20:44:08
release "nginxv1" uninstalled
 Deepak S   deepaks      


 Deepak S   deepaks    kubectl get pods                                                                               in pwsh at 20:44:59
NAME                               READY   STATUS    RESTARTS   AGE
prometheus-alertmanager-0          1/1     Running   0          123m
prometheus-server-9965bf77-kvv2p   1/1     Running   0          123m
 Deepak S   deepaks       
 Deepak S   deepaks    helm uninstall prometheus                                                                      in pwsh at 20:46:25
release "prometheus" uninstalled
 Deepak S   deepaks                                                                                                   in pwsh at 20:46:32

 Deepak S   deepaks    helm list                                                                                      in pwsh at 20:46:32
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
 Deepak S   deepaks    kubectl get pods                                                                               in pwsh at 20:46:59
No resources found in default namespace.
 Deepak S   deepaks         
```
During the uninstallation we need to use name of the release



How can we bundle application as helm charts and how to deploy it as helm charts in application or share with others and they can deploy as helm chart in cluster.

Ecommerce application - payment,shipment microserices

create helmchart for payment
create helmchart for shipment
add both chart to repository
others can also can use the chart from repo
repository --> bestcommerce
payment --> chart
shipment --> chart

```
 Deepak S   best-commerce    pwd                                                                                      in pwsh at 15:43:10

Path
----
Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce

 Deepak S   best-commerce    ls                                                                                       in pwsh at 15:43:23

        Directory: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        30-08-2025  03:43 PM                  payments
d----        30-08-2025  03:32 PM                  shipping

 Deepak S   best-commerce        
```

helm create will create complete folder structre for us


```

 Deepak S   best-commerce    helm create payments                                                                     in pwsh at 15:43:25
Creating payments
 Deepak S   best-commerce    helm create shipping                                                                     in pwsh at 15:44:53
Creating shipping
 Deepak S   best-commerce    cd .\payments\                                                                           in pwsh at 15:45:00
 Deepak S   payments    ls                                                                                            in pwsh at 15:45:04

        Directory: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\payments


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        30-08-2025  03:44 PM                  charts
d----        30-08-2025  03:44 PM                  templates
-----        30-08-2025  03:44 PM            349   .helmignore
-----        30-08-2025  03:44 PM           1144 󰉢  Chart.yaml
-----        30-08-2025  03:44 PM           4294 󰉢  values.yaml

 Deepak S   payments    cd ..\shipping\                                                                               in pwsh at 15:45:06
 Deepak S   shipping    ls                                                                                            in pwsh at 15:45:14

        Directory: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\shipping


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        30-08-2025  03:45 PM                  charts
d----        30-08-2025  03:45 PM                  templates
-----        30-08-2025  03:45 PM            349   .helmignore
-----        30-08-2025  03:44 PM           1144 󰉢  Chart.yaml
-----        30-08-2025  03:44 PM           4294 󰉢  values.yaml

 Deepak S   shipping         

```

charts folder for dependencies what we want we to install along with application

chart.yaml --> metadata information for chart owner,version, version of application deployed on chart
values.yaml -->  any customisation to the template UAT 2 replica
Prodcution 3 customise whatever files added on template
templates --> prvoide deployment yaml file or stateful set or daemonset file service account file configmao file , all the yaml file resoucres , bundle of resources

```

 Deepak S   payments    cd .\templates\                                                                               in pwsh at 15:54:35
 >  ls

        Directory: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\payments\templates


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        30-08-2025  03:44 PM                󰙨  tests
-----        30-08-2025  03:44 PM           1792   _helpers.tpl
-----        30-08-2025  03:44 PM           2385 󰉢  deployment.yaml
-----        30-08-2025  03:44 PM            994 󰉢  hpa.yaml
-----        30-08-2025  03:44 PM           1091 󰉢  ingress.yaml
-----        30-08-2025  03:44 PM           1748 󰈙  NOTES.txt
-----        30-08-2025  03:44 PM            364 󰉢  service.yaml
-----        30-08-2025  03:44 PM            391 󰉢  serviceaccount.yaml

 Deepak S   templates    pwd                                                                                          in pwsh at 15:54:49

Path
----
Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\payments\templates

```

deployment file with variables so that we can customise with values yaml 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
          command: ['sh', '-c', 'echo {{ .Values.appMessage }}; sleep 3600']
          imagePullPolicy: {{ .Values.image.pullPolicy }}
```
values.yaml
```
image:
  repository: busybox
  tag: latest
  pullPolicy: IfNotPresent
appMessage: "Payments Service"
```
chart.yaml only update version since all other metadata information is correct 

Create chart from above files
helm package shipping
helm pacakage payment

```
 Deepak S   best-commerce    helm package shipping                                                                    in pwsh at 16:40:23
Successfully packaged chart and saved it to: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\shipping-0.1.0.tgz
 Deepak S   best-commerce    helm package payments                                                                    in pwsh at 16:40:33
Successfully packaged chart and saved it to: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce\payments-0.1.0.tgz
 Deepak S   best-commerce       

```


```
 Deepak S   best-commerce    helm repo index .                                                                        in pwsh at 16:41:30
 Deepak S   best-commerce    ls                                                                                       in pwsh at 16:41:47

        Directory: \\wsl.localhost\Ubuntu-18.04\home\deepaks\best-commerce


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d----        30-08-2025  03:44 PM                  payments
d----        30-08-2025  03:45 PM                  shipping
-----        30-08-2025  04:41 PM            709 󰉢  index.yaml
-----        30-08-2025  04:40 PM           2744   payments-0.1.0.tgz
-----        30-08-2025  04:40 PM           2740   shipping-0.1.0.tgz

 Deepak S   best-commerce              
```

```
apiVersion: v1
entries:
  payments:
  - apiVersion: v2
    appVersion: 1.0.0
    created: "2025-08-30T16:41:47.1549735+05:30"
    description: A Helm chart for Kubernetes
    digest: dcf6bb0e731d343e7a0141ccf02922867e8336f5299af0e29e1d4068cc4a559c
    name: payments
    type: application
    urls:
    - payments-0.1.0.tgz
    version: 0.1.0
  shipping:
  - apiVersion: v2
    appVersion: 1.0.0
    created: "2025-08-30T16:41:47.1630053+05:30"
    description: A Helm chart for Kubernetes
    digest: 6dc7410681c3a0afdb81f6e7d39b05670546855752117f0c1fc2a64da0e3c7c4
    name: shipping
    type: application
    urls:
    - shipping-0.1.0.tgz
    version: 0.1.0
generated: "2025-08-30T16:41:47.1503257+05:30"

```

index.yaml fileThis file serves as a catalog of all the charts within the repository, providing metadata about each chart, including its name, version, and location.


To create repo we ca use helm repo add command
artifactory or nexus or github pages

These are values.yaml variables, not using  hard coded values we are updating values

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=my-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --version 1.13.0
```
how to check how values can be set using chart name

```
 Deepak S   best-commerce    helm show values eks/aws-load-balancer-controller | grep cluster                         in pwsh at 17:28:20
# Please keep in mind that the controller pods have `priorityClassName: system-cluster-critical`, enabling HPA may lead to the eviction of other low-priority pods in the node
priorityClassName: system-cluster-critical
# control how Pods are spread across your cluster among failure-domains such as regions, zones,
# The name of the Kubernetes cluster. A non-empty value is required
clusterName:
# cluster contains configurations specific to the kubernetes cluster
cluster:
  dnsDomain: cluster.local
# The AWS region for the kubernetes cluster. Set to use KIAM or kube2iam for example.
# The VPC ID for the Kubernetes cluster. Set this manually when your pods are unable to use the metadata service to determine this automatically
# extraVolumeMounts are the additional volume mounts. This enables setting up IRSA on non-EKS Kubernetes cluster
# clusterSecretsPermissions lets you configure RBAC permissions for secret resources
clusterSecretsPermissions:
  # allowAllSecrets allows the controller to access all secrets in the cluster.
# serviceTargetENISGTags specifies AWS tags, in addition to the cluster tags, for finding the target ENI SG to which to add inbound rules from NLBs.
 Deepak S   best-commerce                         
