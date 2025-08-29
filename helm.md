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

