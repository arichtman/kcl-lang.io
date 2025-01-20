---
id: guide
sidebar_label: Quick Start
---

# Introduction

This guide shows you how to use the KCL language and CLIs to complete the deployment of an application running in Kubernetes. We call the abstraction of application operation and maintenance configuration as `Server`, and its instance as `Application`. It is essentially an operation and maintenance model defined by KCL.

In actual production, the application online generally needs to update several k8s resources:

- Namespace
- Deployment
- Service

This guide requires you to have a basic understanding of Kubernetes. If you are not familiar with the relevant concepts, please refer to the links below:

- [Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
- [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Service](https://kubernetes.io/docs/concepts/services-networking/service/)

## Prerequisites

Before we start, we need to complete the following steps:

1. Install [kcl](https://kcl-lang.io/docs/user_docs/getting-started/install/)

2. Clone the [Konfig repo](https://github.com/kcl-lang/konfig.git)

```shell
git clone https://github.com/kcl-lang/konfig.git && cd konfig
```

## Quick Start

### 1. Compiling

The programming language of the project is KCL, not JSON/YAML which Kubernetes recognizes, so it needs to be compiled to get the final output.

Enter stack dir `examples/appops/nginx-example/dev` and compile:

```bash
cd examples/appops/nginx-example/dev && kcl run -D appenv=dev
```

The output YAML is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sampleapp-dev
  namespace: sampleappns-dev
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: sampleapp
      app.kubernetes.io/env: dev
      app.kubernetes.io/instance: sampleapp-dev
      app.k8s.io/component: sampleapp-dev
  template:
    metadata:
      labels:
        app.kubernetes.io/name: sampleapp
        app.kubernetes.io/env: dev
        app.kubernetes.io/instance: sampleapp-dev
        app.k8s.io/component: sampleapp-dev
    spec:
      containers:
      - env:
        - name: MY_ENV
          value: MY_VALUE
        image: nginx:1.7.8
        name: main
        ports:
        - containerPort: 80
          protocol: TCP
        resources:
          limits:
            cpu: '100m'
            memory: '100Mi'
            ephemeral-storage: '1Gi'
          requests:
            cpu: '100m'
            memory: '100Mi'
            ephemeral-storage: '1Gi'
        volumeMounts: []
---
apiVersion: v1
kind: Namespace
metadata:
  name: sampleappns-dev
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: sampleappns-dev
spec:
  ports:
  - nodePort: 30201
    port: 80
    targetPort: 80
  selector:
    app.kubernetes.io/name: sampleapp
    app.kubernetes.io/env: dev
    app.kubernetes.io/instance: sampleapp-dev
    app.k8s.io/component: sampleapp-dev
  type: NodePort
```

After compiling, we can see three resources:

- A `Deployment` with the name `sampleapp-dev`
- A `Namespace` with the name `sampleappns-dev`
- A `Service` with the name `nginx`

### 2. Modification

The `image` attribute in the `Server` model is used to declare the application's container image. We can modify the `image` value in `base/main.k` to modify or upgrade the image:

```diff
14c14
<     image = "nginx:1.7.8"
---
>     image = "nginx:latest"
```

Recompile the configuration code to obtain the modified YAML output:

```shell
kcl run -D appenv=dev
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sampleapp-dev
  namespace: sampleappns-dev
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: sampleapp
      app.kubernetes.io/env: dev
      app.kubernetes.io/instance: sampleapp-dev
      app.k8s.io/component: sampleapp-dev
  template:
    metadata:
      labels:
        app.kubernetes.io/name: sampleapp
        app.kubernetes.io/env: dev
        app.kubernetes.io/instance: sampleapp-dev
        app.k8s.io/component: sampleapp-dev
    spec:
      containers:
      - env:
        - name: MY_ENV
          value: MY_VALUE
        image: nginx:1.7.8
        name: main
        ports:
        - containerPort: 80
          protocol: TCP
        resources:
          limits:
            cpu: '100m'
            memory: '100Mi'
            ephemeral-storage: '1Gi'
          requests:
            cpu: '100m'
            memory: '100Mi'
            ephemeral-storage: '1Gi'
        volumeMounts: []
---
apiVersion: v1
kind: Namespace
metadata:
  name: sampleappns-dev
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: sampleappns-dev
spec:
  ports:
  - nodePort: 30201
    port: 80
    targetPort: 80
  selector:
    app.kubernetes.io/name: sampleapp
    app.kubernetes.io/env: dev
    app.kubernetes.io/instance: sampleapp-dev
    app.k8s.io/component: sampleapp-dev
  type: NodePort
```

## Resources

- More examples can be found [here](https://github.com/kcl-lang/konfig/blob/main/examples/README.md)
- Konfig schema reference document can be found [here](https://github.com/kcl-lang/konfig/blob/main/docs/konfig.md)

## Summary

This document mainly introduces how to use the KCL and Konfig library to deploy a Long Running application running in Kubernetes.
