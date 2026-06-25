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

Q6 - 
