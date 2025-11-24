# Introduction to Kubernetes (K8s)

Kubernetes is a powerful **container orchestrator**. While Docker is used to create and run individual containers, Kubernetes is used to manage hundreds or thousands of them across a cluster of servers automatically.


## 1. Why Use Kubernetes? (The "Magic" Features)

Kubernetes takes the manual work out of managing infrastructure by providing:

* **Self-Healing:** If a container crashes, Kubernetes kills it and starts a new one instantly. If a whole server (node) fails, it moves all containers to a healthy server.
* **Auto-Scaling:** It can add more copies of your app (Pods) if traffic spikes, and shrink them back down when traffic is low.
* **Automatic Bin Packing:** You tell K8s how much CPU/RAM your app needs, and it automatically finds the best server to fit it in, like a game of high-stakes Tetris.
* **Service Discovery & Load Balancing:** It gives your app a single "entry point" (IP/DNS) and automatically spreads incoming traffic across all running copies of your app.


## 2. Kubernetes Architecture: The Brain and the Muscle

A Kubernetes cluster is divided into two main parts: the **Control Plane** (The Brain) and **Worker Nodes** (The Muscle).

### The Control Plane (Master Node)

This node doesn't run your apps; it makes all the decisions.

1. **API Server:** The "front desk." Everything (like your `kubectl` commands) goes through here first.
2. **etcd:** The "brain's memory." A database that stores every single detail about the cluster. It is the **source of truth**.
3. **Scheduler:** The "planner." It looks at a new Pod and decides which Worker Node has enough "room" (CPU/RAM) to run it.
4. **Controller Manager:** The "policeman." It constantly checks: *"Do I have 3 copies running? Yes? Good. No? Create one now."*

### The Worker Nodes

These are the servers where your applications (Pods) actually live and work.

1. **Kubelet:** The "captain" of the node. it takes orders from the API Server and makes sure the containers are running as requested.
2. **Kube-Proxy:** The "network manager." It handles the IP addresses and ensures traffic gets to the right container.
3. **Container Runtime:** The "engine" (like `containerd`) that actually pulls the image and starts the container.


## 3. What is a Pod?

A **Pod** is the smallest unit you can create in Kubernetes.

* Think of it as a "wrapper" around one or more containers.
* Containers inside the same Pod share the same IP address and can talk to each other via `localhost`.
* **Analogy:** If a container is a person, a Pod is a house. People inside the house share the same address and resources.


## 4. The Lifecycle of a Request: How a Pod is Born

When you run `kubectl run my-pod --image=nginx`, here is the 10-step journey:

1. **Request:** `kubectl` sends a message to the **API Server**.
2. **Check:** API Server verifies who you are (Auth) and if the command is valid.
3. **Record:** API Server writes "I need an Nginx pod" into **etcd**.
4. **Watch:** The **Scheduler** sees a new pod in etcd that has no node assigned yet.
5. **Plan:** Scheduler picks the best Worker Node and tells the API Server.
6. **Update:** API Server updates **etcd** with the node's name.
7. **Order:** The **Kubelet** on that specific Worker Node sees the update and says, "Hey, that's for me!"
8. **Execute:** Kubelet tells the **Container Runtime** (containerd) to pull the image and start the container.
9. **Network:** **Kube-proxy** sets up the networking so the pod can communicate.
10. **Final State:** The Pod is now "Running," and the **Controller** keeps an eye on it to make sure it stays that way.


## 5. Essential Commands

* **Check cluster health:** `kubectl get nodes`
* **Launch a pod:** `kubectl run <name> --image=<image>`
* **See your pods:** `kubectl get pods`
* **Get "under the hood" details:** `kubectl describe pod <name>`
* **Delete a pod:** `kubectl delete pod <name>`