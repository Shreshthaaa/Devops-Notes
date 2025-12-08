# Kubernetes Services: Networking and Load Balancing

In Kubernetes, a **Service** is an abstraction that defines a logical set of Pods and a policy by which to access them. They provide stable networking for the otherwise temporary and changing world of Pods.


## 1. Why We Need Services

Pods are **ephemeral**—they are created and destroyed frequently. When a Pod is recreated, it receives a **new IP address**. Relying on these shifting IPs for communication is impossible.

A Service provides:

* **Stable Identity:** A fixed IP address and DNS name that never changes.
* **Load Balancing:** Spreading traffic across all Pods that match a specific label.
* **Service Discovery:** Automatically tracking which Pods are healthy and ready to receive traffic.


## 2. Key Concepts: Ports and Selectors

### Labels and Selectors

A Service finds its target Pods using **Selectors**. If a Pod has the label `app: backend`, and the Service selector is `app: backend`, the Service will automatically send traffic to that Pod.

### Understanding the Port Flow

There are three main port definitions to remember:

1. **Port:** The port number exposed by the **Service** (the virtual IP).
2. **TargetPort:** The port number on the **Pod container** where the app is listening.
3. **NodePort:** A specific port (30000–32767) opened on **every Node** to allow external access.

**Flow:** `Client` → `Service IP : Port` → `Pod IP : TargetPort`


## 3. Kubernetes Service Types

Kubernetes offers different types of services depending on whether you need internal or external access.

### Type 1: ClusterIP (Default)

Exposes the Service on a cluster-internal IP. This makes the Service only reachable from within the cluster.

* **Use Case:** Internal microservice communication (e.g., Backend talking to a Database).
* **DNS Name:** `<service-name>.<namespace>.svc.cluster.local`

### Type 2: NodePort

Exposes the Service on each Node’s IP at a static port (30000–32767).

* **Mechanism:** It automatically creates a ClusterIP internally.
* **Access:** You can reach the service by hitting `<NodeIP>:<NodePort>`.
* **Use Case:** Development environments or bare-metal clusters where cloud load balancers aren't available.

### Type 3: LoadBalancer

Used in cloud environments (AWS, GCP, Azure).

* **Mechanism:** It creates a NodePort and ClusterIP internally, then tells the cloud provider to provision a professional load balancer.
* **Access:** You get a single `EXTERNAL-IP` or DNS name (like an AWS ELB).
* **Use Case:** Production internet traffic.

### Type 4: ExternalName

Maps a Service to a DNS name instead of a Pod selector.

* **Use Case:** Allowing Pods to talk to an external database (e.g., RDS) using a local Kubernetes service name.


## 4. Internal Components: kube-proxy and DNS

### kube-proxy

The **kube-proxy** is a network agent that runs on every node. It doesn't actually "touch" the traffic; instead, it writes rules (usually `iptables` or `IPVS`) into the Linux kernel. These rules tell the node: *"If traffic comes for Service IP X, redirect it to Pod IP Y."*

### CoreDNS

Kubernetes has a built-in DNS service called **CoreDNS**. It watches the API server for new Services and automatically creates DNS records. This is why you can `curl http://my-service` instead of using an IP address.


## 5. Summary Cheat Sheet

| Feature | ClusterIP | NodePort | LoadBalancer |
| --- | --- | --- | --- |
| **Reachability** | Internal Only | External (via Node IP) | External (via Cloud IP) |
| **Port Range** | Any | 30000–32767 | Any |
| **Cloud Integration** | No | No | Yes |
| **Typical Use** | Backend communication | Local testing | Production web apps |