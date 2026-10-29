Kubernetes Services

A Kubernetes Service provides a stable network endpoint to access a group of Pods. It enables communication between applications within the cluster and allows applications to be exposed externally when required.

Kubernetes has four main types of Services:

ClusterIP – Exposes a Service internally within the cluster.
NodePort – Exposes a Service through a static port on each node.
LoadBalancer – Exposes a Service through an external load balancer.
ExternalName – Maps a Service name to an external DNS hostname.
1. NodePort Service

Allows external access to an application by exposing a static port on each node. The default NodePort range is 30000–32767.

Example
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-portal
spec:
  type: NodePort
  selector:
    app: grade-submission-portal
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080

How It Works
An external request is initiated using the node's IP address and static port (nodePort: 30080).
The request enters the cluster through the NodePort and is routed to the Service's internal port (port: 8080). The Service port can be any valid port number, independently of the NodePort.
The Service selects matching Pods using the label selector (app: grade-submission-portal).
The request is forwarded to the application port inside the Pod (targetPort: 8080).

Note: NodePort is useful for development, testing, and prototyping, but is less commonly used directly for production exposure.

2. ClusterIP Service

Used for internal communication between applications within the Kubernetes cluster. It is the default Service type.

Example
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
    - port: 8080
      targetPort: 8080

How It Works
Pods within the cluster can access the Service using its DNS name (backend-service) and Service port (port: 8080), provided they use the appropriate namespace.
The Service selects matching Pods using the label selector (app: backend).
The Service forwards requests to the application port inside the Pod (targetPort: 8080).

Note: ClusterIP is commonly used for internal communication, such as frontend-to-backend requests and backend-to-database connections.

3. LoadBalancer Service

Exposes an application externally through a load balancer, typically provisioned by a cloud provider.

Example
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-portal
spec:
  type: LoadBalancer
  selector:
    app: grade-submission-portal
  ports:
    - port: 8080
      targetPort: 8080

How It Works
An external client sends a request to the load balancer's assigned IP address or hostname on the exposed port (port: 8080).
The load balancer forwards the request to the Kubernetes Service.
The Service selects matching Pods using the label selector (app: grade-submission-portal).
The request is forwarded to the application port inside the Pod (targetPort: 8080).

Note: LoadBalancer is commonly used to expose applications to external users in production. It requires a compatible load-balancer implementation.

4. ExternalName Service

Maps a Kubernetes Service name to an external DNS hostname using a CNAME record. Unlike the other Service types, it does not select Pods or proxy traffic.

Example
apiVersion: v1
kind: Service
metadata:
  name: external-database
spec:
  type: ExternalName
  externalName: db.example.com

How It Works
Applications within the cluster access the external dependency using the Kubernetes Service's DNS name.
Kubernetes DNS resolves the Service name to the external hostname (db.example.com) through a CNAME record.
The application connects directly to the external destination using the appropriate protocol and port.

Note: ExternalName is useful for connecting to external databases, legacy systems, or services hosted outside the Kubernetes cluster.

Comparison of Kubernetes Service Types
Feature	ClusterIP	NodePort	LoadBalancer	ExternalName
Primary purpose	Internal communication	Access through node IP and port	External load balancing	External DNS alias
Internal access	Yes	Yes	Yes	DNS-based
External access	Not directly	Yes	Yes, if configured	DNS alias only
Selects Pods	Yes	Yes	Yes	No
Typical use	Internal APIs	Testing and development	Public applications	External dependencies
Key Takeaways
ClusterIP: Used for internal communication within the cluster.
NodePort: Exposes a Service through a static port on each node.
LoadBalancer: Exposes a Service through an external load balancer.
ExternalName: Maps a Service name to an external DNS hostname.

Additional Note: Ingress is a separate Kubernetes resource used for HTTP/HTTPS routing. It is not a fifth Service type.