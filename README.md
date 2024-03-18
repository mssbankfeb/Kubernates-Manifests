1. Pods and Containers: Pods are the smallest deployable units in Kubernetes, each encapsulating one or more containers. They are ephemeral by nature, which means understanding their lifecycle and behavior is key. Containers within a Pod share the same network space and storage, allowing them to communicate and share data more easily. Here's a basic example of a Pod manifest:
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: nginx
2. Deployments and ReplicaSets: Deployments manage the desired state of your application, ensuring that the specified number of Pods are running at any given time. They provide powerful features like rolling updates and rollback capabilities. ReplicaSets, a subset of Deployments, ensure that a specified number of Pod replicas are running at all times. An example Deployment manifest could look like this:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
3. Services: In Kubernetes, a Service is an abstraction layer that defines a logical set of Pods and a policy by which to access them. This decouples work definitions from the pods. Services allow your applications to receive traffic, and they can be exposed in different ways (ClusterIP, NodePort, LoadBalancer, etc.). Here's a basic Service configuration:
apiVersion: v1
kind: Service
metadata:
  name: example-service
spec:
  selector:
    app: example
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
4. Namespaces: Namespaces are like virtual clusters within a Kubernetes cluster. They help in organizing resources and can be seen as a way to divide cluster resources between multiple users. They are particularly useful in environments with many users across multiple teams or projects. Here's how a Namespace is defined:
apiVersion: v1
kind: Namespace
metadata:
  name: example-namespace
5. ConfigMaps and Secrets: ConfigMaps and Secrets are Kubernetes objects used to manage non-sensitive and sensitive data, respectively. ConfigMaps allow you to decouple environment-specific configuration from your container images, making your applications portable. Secrets are similar but are used to store sensitive information, like passwords or API keys. Below are examples of both:
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
data:
  config.json: |
    {
      "database": "mysql",
      "databasePath": "/var/lib/mysql"
    }
apiVersion: v1
kind: Secret
metadata:
  name: example-secret
type: Opaque
data:
  password: cGFzc3dvcmQ=
6. Persistent Volumes (PV) and Persistent Volume Claims (PVC): PVs and PVCs are crucial for stateful applications that require persistent storage. A PV is a piece of storage in the cluster, while a PVC is a request for storage by a user. They abstract the details of how the storage is provided and how it's consumed. Here are their respective manifests:
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: slow
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
**7. Ingress Controllers and Ingress Resources:** Ingress resources manage external access to the services in a cluster, typically HTTP. Ingress may provide load balancing, SSL termination, and name-based virtual hosting. An Ingress Controller is responsible for fulfilling the Ingress, often with a load balancer. A simple Ingress resource looks like this:
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: www.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: example-service
            port:
              number: 80
**8. Resource Quotas and Limit Ranges:** Resource Quotas and Limit Ranges help manage the consumption of resources in a Kubernetes cluster. While Resource Quotas set constraints on the overall resource use per Namespace, Limit Ranges specify constraints on resource use per Pod or Container. These tools are essential for maintaining the health and efficiency of a Kubernetes cluster. Examples include:
apiVersion: v1
kind: ResourceQuota
metadata:
  name: example-quota
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
apiVersion: v1
kind: LimitRange
metadata:
  name: example-limitrange
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
    
**9. Network Policies:** Network Policies are key for securing Kubernetes networks. They define how groups of Pods can communicate with each other and with other network endpoints. Network Policies are crucial in enforcing a secure microservice architecture. Here’s an example of a Network Policy:
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example-network-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
10. Helm Charts: Helm is a package manager for Kubernetes, which simplifies the deployment and management of applications on Kubernetes clusters. Helm Charts help you define, install, and upgrade even the most complex Kubernetes applications. While a Helm Chart doesn't have a simple manifest like the other objects, understanding its structure and usage is vital for efficient Kubernetes deployments.
