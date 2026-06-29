# SRE-Interview-Prep
SRE Interview Preparation 

K8s and OpenShift Related Questions - 

Q1 - Explain Lifetime of a Pod or container.

Answer - Pod lifecycle starts with creation, then it goes to Pending while Kubernetes schedules it to a node and prepares image/network/storage. After the container starts, the pod becomes Running. Probes like readiness and liveness monitor whether the app is ready and healthy. If the container crashes, Kubernetes restarts it based on restart policy. When the pod is deleted, Kubernetes sends SIGTERM for graceful shutdown, waits for the grace period, and then force kills it if needed.

Q2 - What is readiness and liveliness probe and where to do you define it?

Answer - Readiness probe checks whether the application is ready to accept traffic. If it fails, the pod stays running but is removed from service endpoints, so no traffic is sent to it. Liveness probe checks whether the application is still alive and healthy. If it fails, Kubernetes restarts the container. Both probes are configured inside the container section of the pod or deployment manifest, usually under spec.template.spec.containers.

Q3 - Explain important features of Kubernetes and Openshift.

Answer - Kubernetes is mainly used for orchestrating containerized applications. Its key features are scheduling workloads across nodes, self-healing by restarting failed containers or recreating pods, auto-scaling, service discovery and load balancing, rolling updates, persistent storage support, and declarative desired-state management through YAML. It also supports health monitoring through readiness and liveness probes, and config management through ConfigMaps and Secrets.
OpenShift is built on Kubernetes but adds enterprise features on top of it. Some important OpenShift features are its built-in web console, stronger default security using SCCs and non-root execution, Routes for exposing services externally, integrated image registry, BuildConfig and Source-to-Image for building images, and a strong Operator ecosystem for lifecycle management of platform services and applications. So in short, Kubernetes is the orchestration engine, and OpenShift is a more enterprise-ready platform built around it.

Q4 - What is headless service?

Answer - Headless service means a service without ClusterIP. It doesn’t load balance through one virtual IP; instead it exposes individual pod IPs through DNS. It is mainly used for StatefulSets and stateful distributed applications when we need direct pod to pod communication.

Q5 - What are Stateless and statefull applications?

Answer - Stateless applications do not store important state in the pod, so any replica can serve requests and they are easy to scale using Deployments. Stateful applications need persistent data and stable identity, so they usually run using StatefulSets with persistent volumes. Web frontends and APIs are common stateless examples, while databases and Kafka are common stateful examples.

Q6 - When a k8s cluster is set up, what content does default namespace have?

Answer - Usually nothing much except the default service account. It is the namespace used for resources created without specifying a namespace. Kubernetes system components are generally not in default; they are mainly in the kube-system namespace.

Q7 - What are CRDs?

Answer - A Custom Resource Definition (CRD) is a Kubernetes feature that extends the Kubernetes API by allowing us to define new resource types. Once a CRD is installed, we can create Custom Resources (CRs) of that type, and an Operator can watch those resources to automate tasks like deployment, scaling, upgrades, and backups. CRDs are widely used by Operators to manage complex applications such as databases, Kafka, and Elasticsearch.

Q8 - Replicaset and replication controller?

Answer - Both ReplicaSet and ReplicationController ensure that the desired number of pod replicas are running. If a pod fails or is deleted, they automatically create a new one. ReplicationController is the older controller and only supports equality-based label selectors. ReplicaSet is its successor and supports both equality-based and set-based selectors, making it more flexible. In modern Kubernetes, ReplicaSets are typically managed by Deployments rather than being created directly.

Q9 - what is blue green deployment?

Answer - Blue-Green Deployment is a deployment strategy where you maintain two identical environments:

Blue → Current production version
Green → New version
Only one environment receives production traffic at a time.
When the new version is tested and verified, traffic is switched from Blue to Green.

Q10 - What is canary deployment?

Answer - A Canary Deployment is a deployment strategy where the new version is released to a small percentage of users first. If everything works well, traffic is gradually increased until all users are using the new version. Unlike Blue-Green, where 100% of traffic switches at once, Canary shifts traffic gradually.

Q11 - How will you access a particular pod of statefulset?

Answer - Each pod in a StatefulSet has a stable hostname and network identity. To access a specific pod, we typically use a Headless Service. Kubernetes DNS creates a unique DNS entry for each pod in the format:
<pod-name>.<headless-service>.<namespace>.svc.cluster.local
This allows applications to communicate with a specific pod instead of going through load balancing.

Q12 - Difference between deployment and statefulset?

Answer - A Deployment is used for stateless applications where pod identity is not important. It manages ReplicaSets to ensure the desired number of replicas are running and supports rolling updates and scaling. If a pod fails, a new pod with a different name is created, and any pod can handle incoming requests.

A StatefulSet is used for stateful applications that require stable network identities and persistent storage. Each pod has a predictable name, such as mysql-0 or mysql-1, and is associated with its own Persistent Volume. If a pod is recreated, it retains the same identity and storage. StatefulSets are commonly used for databases, Kafka, and other distributed systems, often together with a Headless Service.

Q13 - HPA and VPA?

Answer - Horizontal Pod Autoscaler (HPA) automatically scales the number of pod replicas based on metrics like CPU, memory, or custom metrics. It is mainly used to handle changes in workload by scaling applications out or in. Vertical Pod Autoscaler (VPA), on the other hand, adjusts the CPU and memory requests of existing pods based on their resource usage. HPA changes the number of pods, while VPA changes the size of each pod. HPA is commonly used for stateless applications experiencing varying traffic, whereas VPA helps optimize resource allocation for applications whose resource requirements change over time.

Q14 - What is serviceaccount in K8s and Openshift?

Answer - A ServiceAccount is an identity used by applications running inside Kubernetes pods. When a pod needs to communicate with the Kubernetes API server, it authenticates using the ServiceAccount assigned to it. Permissions are controlled through RBAC using Roles or ClusterRoles and RoleBindings or ClusterRoleBindings. Every namespace has a default ServiceAccount, but we usually create dedicated ServiceAccounts for applications to follow the principle of least privilege. In OpenShift, ServiceAccounts are also used by components such as builders, deployers, and Operators to perform automated tasks securely.

Q13 - What is ingress and egress?

Answer - Ingress is the mechanism for routing incoming external traffic into a Kubernetes cluster. It allows users to access applications using hostnames or URL paths and forwards requests to the appropriate Service. In OpenShift, this functionality is commonly provided by Routes. Egress refers to traffic leaving the cluster, such as when a pod connects to an external API, database, or third-party service. In short, Ingress is incoming traffic to the cluster, while Egress is outgoing traffic from the cluster.

Q14 - Difference between Ingress resource and Ingress Controller?

Answer - Ingress is a Kubernetes resource (YAML) that defines routing rules (hostnames, paths, backend Services).
Ingress Controller is the software (such as NGINX or HAProxy) that watches Ingress resources and actually configures the load balancer or proxy to route the traffic.

Without an Ingress Controller, creating an Ingress resource alone does not expose your application.

Q15 - How you will restrict users from access ingress resource.

Answer - 1. Restrict users from creating or modifying Ingress resources (RBAC)

To restrict users from creating, editing, or deleting Ingress resources, I would use RBAC. I would create a Role or ClusterRole without permissions on the ingresses resource and bind it to the appropriate users or groups using RoleBindings or ClusterRoleBindings.

2. Restrict external users from accessing an application through Ingress

Suppose you want only employees to access: Authentication (OAuth, OIDC, LDAP), IP allowlists, WAF, TLS with client certificates, Network firewall, Ingress Controller annotations

Q16 - Difference between load balancer and node port service?

Answer - A NodePort Service exposes an application on every worker node using a port in the range 30000–32767. Clients access the application using the node's IP address and the assigned port. It is simple and works on any Kubernetes cluster but is generally used for development or testing.

A LoadBalancer Service, on the other hand, provisions or integrates with an external load balancer that provides a stable external IP address and distributes incoming traffic across the cluster. It is the preferred option for exposing production applications on cloud platforms. In OpenShift, applications are most commonly exposed using Routes rather than directly using NodePort or LoadBalancer Services.

Q17 - Types of services?

Answer - A Service is a Kubernetes object that provides a stable network connection to access a group of Pods. There are 4 types of services -

a. ClusterIP - It exposes the application only within the Kubernetes cluster.
b. NodePort - A NodePort exposes the application on every worker node.
c. Loadbalancer - A LoadBalancer exposes the application using an external load balancer.
d. ExternalName - This Service doesn't point to Pods. Instead, it maps a Kubernetes Service name to an external DNS name.

Q18 - If OpenShift uses Routes, what Service type is usually behind a Route?

Answer - A ClusterIP Service. The Route receives external traffic and forwards it to the ClusterIP Service, which then load-balances traffic to the application Pods. This is the standard pattern in OpenShift.

Q19 - Different types of Routes?

Answer - There are 4 types of routes - a. Edge Route, b. Passthrough, c. Re-encrypt, d. HTTP/Insecure

<img width="277" height="125" alt="image" src="https://github.com/user-attachments/assets/d093a3c0-2d68-4b6a-a6c6-8ac1b0e310b2" />

OpenShift mainly supports three TLS route types: Edge, Passthrough, and Re-encrypt, plus plain HTTP routes. Edge termination is the most common and terminates TLS at the OpenShift router. Passthrough sends encrypted traffic directly to the application pod, so the pod handles TLS. Re-encrypt terminates TLS at the router and then establishes a new TLS connection to the pod, providing encryption both externally and internally. HTTP routes do not use TLS and are generally used for non-production or internal testing.

Q20 - What is Node Affinity and Node Anti affinity?

Answer - Node Affinity and Node Anti-Affinity are scheduling rules that tell Kubernetes where a Pod should or should not run based on node labels.

Node Affinity tells the scheduler: "Schedule this Pod only on nodes that match certain labels." Think of it as attracting a Pod to specific nodes. Types of Node Affinity -

1. RequiredDuringSchedulingIgnoredDuringExecution - This is a hard rule, if it doesn't match pod will be in pending state
2. PreferredDuringSchedulingIgnoredDuringExecution - This is a soft rule, if scheduler doesn't find a match, it schedules the Pod elsewhere.

Node Anti-Affinity : Kubernetes does not have a separate feature called "Node Anti-Affinity." For nodes, you achieve anti-affinity by using Node Affinity with the NotIn or DoesNotExist operators.

Note: Node Affinity is different from Node Selector: Node selector works with key value match but node affinity is more flexible besides key value match, it also supports operators like - in, Notin, Exists, Doesnotexist and more. 

Q21 - What is Pod Affinity and Anti-Affinity?

Answer - Pod Affinity: Decides whether a Pod should be placed near another Pod.

Pod Anti-Affinity: Ensures Pods are not scheduled together.

Q22 - 
