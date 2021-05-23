# Namespaces

[Youtube](https://youtu.be/X48VuDVv0do?t=6377)

## What is a Namespace?

Resources can be organized into namespaces. It's a virtual cluster within a kubernetes cluster. The components deployed in a namespace are the system processes, and they're from master and managing processes or `kubectl` etc. 

`kubectl get namespace` is how you retrieve the current namespaces.

### kubernetes-dashboard

Shipped automatically with **minikube**

### kube-system

Not meant for our use. Don't modify or create anything in this namespace.

### kube-public

Contains publicly accessible data like a configmap that contains cluster information.

```
# preview the information available through this namespace
$ kubectl cluster-info
```

> Example of output
```
❯ kubectl cluster-info
Kubernetes master is running at https://192.168.64.2:8443
KubeDNS is running at https://192.168.64.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### kube-node-lease

Holds information about the hearbeat of nodes. Each node has an associated lease object in the namespace that determines the availability of the node.

### default

Where we create our resources in the beginning. You can create your own namespace.

## What are the use cases?

### Create a namespace

`kubectl create named my-namespace` will create a namespace.

```
❯ kubectl get namespace
NAME              STATUS   AGE
default           Active   20d
kube-node-lease   Active   20d
kube-public       Active   20d
kube-system       Active   20d
my-namespace      Active   5s
```

Namespaces can also be creating via configuration files. Here is an example of a yaml file.

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-configmap
  namespace: my-namespace
data:
  db_url: mysql-service.database
```

### Everything in one namespace

Deploying everything within the default namespace. A complex application will cause the namespace to be filled with different components, making it harder to manage.

### Resources grouped in namespaces

Group resources in namespaces. Examples...

- all the database components within a database namespace

- all the monitoring components within a monitoring namespace.

- all elastic stack components within an elastic stack namespace.

- all nginx ingress components within an nginx-ingress namespace.

Kubernetes recommends not using this approach if it's a simple application within 10 or less users.

However, grouping them is beneficial regardless. Not grouping into a namespace can lead to teams overriding configs accidentally for choosing the same name.

Hosting development and staging in one cluster. Re-use those components in both environments, like a blue/green deployment.

You can also limit access to the resources by namespaces. Teams could have their own namespace, and access is limited to them. Additionally, resources like CPU/RAM/Storage can be controlled through quotas. 

## How Namespaces work and how to use it?
