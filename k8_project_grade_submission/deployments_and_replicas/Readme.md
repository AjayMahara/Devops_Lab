A Deployment in Kubernetes allows us to declaratively manage a set of pods. The two key specifications in a Deployment are the number of pod replicas, and the pod template (defining how each pod should be constructed). This Deployment declares our desired state: 3 replicas of pods based on the specified template:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: my-app
        image: my-app:v1

Behind the scenes:

The Deployment Controller creates a ReplicaSet based on this specification.

A ReplicaSet is a Kubernetes resource that ensures a specified number of pod replicas are running at all times.

The ReplicaSet Controller watches for ReplicaSet objects and creates pods based on them.

The ReplicaSet Controller continuously monitors the state of the pods.

If a pod terminates or fails, the ReplicaSet Controller automatically creates a new one to maintain the desired number of replicas.

The Deployment Controller manages the overall lifecycle of the Deployment, including updates and rollbacks (discussed in the next section), while the ReplicaSet Controller handles the day-to-day management of pods. This orchestration happens automatically. As developers, we simply declare our desired state in the Deployment object, and Kubernetes handles the rest, ensuring our application remains running as specified.


![alt text](image.png)