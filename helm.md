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


