# SRE-Interview-Prep
SRE Interview Preparation (K8s and OpenShift)

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

Answer - 
