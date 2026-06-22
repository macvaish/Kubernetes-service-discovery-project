# Kubernetes-service-discovery-project

Suppose we have an internal web appliacation.
Then the application should always be available
1. If pod crashes, the application should be available
2. Application should not depend on pod ip's
3. Internal service should communicate securely.
4. Application should be accessible from outside cluster
5. Traffic should be distributed across multiple pods.

PROBLEM STATEMENT
In Kubernetes, Pods are ephemeral and receive dynamic IP addresses. When a Pod is restarted, recreated, or scaled, its IP address changes. Applications communicating directly using Pod IPs can lose connectivity, causing service disruption and making microservice communication unreliable.

SOLUTION
Implemented an Nginx application using a Kubernetes Deployment with 3 replicas and exposed it through a ClusterIP Service. Kubernetes Service Discovery and CoreDNS were used to provide a stable DNS name and virtual IP for accessing the application. This eliminated dependency on dynamic Pod IPs, enabled internal load balancing, and ensured uninterrupted communication even when Pods were recreated through Kubernetes self-healing.

WHY DEPLOYEMENT?
If pods are created directly the application will fail if the pod crashes and there's no auto recovery.Deployment manages pods through replica sets.

WHY LABELS?
when the pod crashes the replicaset will automatically create a new pod even before the pod is crashed. everytime the new pod is created the ip address of the pod get changed. It is difficult to identify. So we give labels to each pod as identity.

WHY SELECTORS?
labels are attached to resources while selectors are used to identify and select based on labels.

WHY ClLUSTERIP?
Since pod's IP's are dynamic, clusterIP acts as a fixed endpoint for internal communivcation and loadbalance traffic across healthy pods.

Why REPLICAS?
Application should always remain available. even if the pod dies it should create another pod.

WHY SERVICE DISCOVERY?
Pods are ephemeral. Service Discovery is used to enable applications inside Kubernetes to communicate using stable service names instead of dynamic Pod IP addresses.When service is created, kubernetes DNS automatically creates a DNS record.

WHY NODEPORT?
The users can access the application who has the access to node IP. users cannot access pods directly because pods are private. Kubernetes opens a port on every node.

WHY LOAD BALANCING?
when traffic is heavy, kubernetes service distributes incoming requests across all healthy pods.

WHY SELF HEALING?
If a pod is deleted, immediately a new pod is created automatically.
 
Load balancing is not implemented because we are using minikube which needs a cloud provider (AWS, AZURE, GCP)
