While a Kubernetes cluster is physically distributed across multiple machines (master and worker nodes), developers interact with it as a single entity partitioned into logical divisions called namespaces. These namespaces group together closely related resources.

Deploying to a Specific Namespace :
Command-line approach: kubectl apply -f my-deployment.yaml -n my-namespace

In YAML configuration:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: grade-submission-api
  namespace: backend-services
  labels:
    app.kubernetes.io/name: grade-submission
    app.kubernetes.io/component: backend
    app.kubernetes.io/instance: grade-submission-api


Default Namespace : If no namespace is specified, Kubernetes will place the resource in the default namespace.

Best Practices : 
-> Use namespaces to organize and isolate your workloads logically.
-> Be aware of the namespace you're working in to avoid unintended interactions between resources.
-> Collaborate with your cluster administrators to understand any namespace-level policies or quotas that may affect your deployments.


To list all the namespaces use command : kubectl get namespaces

To create new namespace use the command : 
-> kubectl create namespace <namespace_name>
For exp : kubectl create namespace backend-services

To make sure the application uses this namespace add namespace key-value pair to the metadata

metadata:
  name: grade-submission-api
  namespace: backend-services
  labels:
    app.kubernetes.io/name: grade-submission
    app.kubernetes.io/component: backend
    app.kubernetes.io/instance: grade-submission-api
