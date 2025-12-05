# Kubernetes Deployment Deep Dive

A **Deployment** is the standard way to manage stateless applications in production. It provides a declarative layer over Pods and ReplicaSets, adding the "intelligence" required for updates and stability.


## 1. The Deployment Hierarchy

A Deployment does not manage Pods directly. It manages **ReplicaSets**, which in turn manage the **Pods**.

* **Containers:** The actual application process.
* **Pods:** The smallest deployable unit (inherited from ReplicaSet).
* **ReplicaSet:** Ensures the correct number of pod copies are running (inherited from Deployment).
* **Deployment:** Adds versioning, rollouts, and rollbacks.


## 2. Cluster Setup and Networking

Before a Deployment can run, the cluster must be initialized and networked.

* **Initialization:** `kubeadm init` starts the control plane components. Until a **CNI (Container Network Interface)** is installed, the nodes will remain in a `NotReady` state because system pods like `coredns` cannot communicate.
* **Networking:** Installing a plugin like **Weave** creates a DaemonSet that ensures every node has a networking pod to handle IP allocation and pod-to-pod routing.


## 3. Rolling Updates and Zero Downtime

This is the primary reason to use a Deployment over a ReplicaSet.

### How a Rolling Update Works:

When you update a Deployment (e.g., changing the image version):

1. Kubernetes creates a **new ReplicaSet**.
2. It gradually scales **up** the new ReplicaSet.
3. It gradually scales **down** the old ReplicaSet.
4. This ensures that some pods are always available to handle traffic during the transition.

### Update Strategies:

* **RollingUpdate (Default):** Gradually replaces pods. Controlled by `maxUnavailable` (how many pods can be down) and `maxSurge` (how many extra pods can be created).
* **Recreate:** Kills all old pods before starting new ones. This causes downtime but is necessary for some applications that cannot run two versions at once.


## 4. Essential Rollout Commands

Manage the lifecycle of your application versions with these commands:

| Task | Command |
| --- | --- |
| **Update Image** | `kubectl set image deployment/<name> <container>=<new-image>` |
| **Check Progress** | `kubectl rollout status deployment/<name>` |
| **View History** | `kubectl rollout history deployment/<name>` |
| **Undo Update** | `kubectl rollout undo deployment/<name>` |
| **Specific Version** | `kubectl rollout undo deployment/<name> --to-revision=2` |


## 5. Scaling Mechanisms

Deployment scaling can be handled in three ways depending on your needs:

1. **HPA (Horizontal Pod Autoscaler):** Adds more **Pods** when CPU or Memory usage hits a limit.
2. **VPA (Vertical Pod Autoscaler):** Increases the **CPU/RAM resources** assigned to existing pods.
3. **CA (Cluster Autoscaler):** Adds more **Nodes (Servers)** to the cluster if there is no room left for new pods.


## 6. Internal Storage (etcd)

Everything is stored as a "Desired State" in the **etcd** database.

* **Deployments:** `/registry/deployments/default/<name>`
* **ReplicaSets:** `/registry/replicasets/default/<name>-hash`
* **Pods:** `/registry/pods/default/<name>-hash-uniqueid`

When you perform a rollback, Kubernetes simply looks at the revision history in etcd and scales the old ReplicaSet back up while scaling the current one down.


## 7. Summary

* **Pod:** Basic unit; not self-healing.
* **ReplicaSet:** Guarantees pod count; self-healing but lacks update logic.
* **Deployment:** The "Gold Standard" for production; handles updates, rollbacks, and history.
* **Self-Healing:** If a node dies, the Deployment ensures those pods are recreated elsewhere.
* **Declarative:** Always use `kubectl apply -f yaml` to keep your etcd state in sync with your files.