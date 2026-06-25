# SRE-Interview-Prep
SRE Interview Preparation
Q1 - Explain Lifetime of a Pod or container.
Answer - Pod lifecycle starts with creation, then it goes to Pending while Kubernetes schedules it to a node and prepares image/network/storage. After the container starts, the pod becomes Running. Probes like readiness and liveness monitor whether the app is ready and healthy. If the container crashes, Kubernetes restarts it based on restart policy. When the pod is deleted, Kubernetes sends SIGTERM for graceful shutdown, waits for the grace period, and then force kills it if needed.

Q2 - What is readiness and liveliness probe and where to do you define it?
Answer - Readiness probe checks whether the application is ready to accept traffic. If it fails, the pod stays running but is removed from service endpoints, so no traffic is sent to it. Liveness probe checks whether the application is still alive and healthy. If it fails, Kubernetes restarts the container. Both probes are configured inside the container section of the pod or deployment manifest, usually under spec.template.spec.containers.

Q3 - Explain important features of Kubernetes and Openshift.
Answer - 
