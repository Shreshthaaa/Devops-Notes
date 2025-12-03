# Kubernetes ReplicaSet & Pod Management

## 1. Cluster Setup and Initialization

Setting up a minimal Kubernetes cluster involves configuring a **Control Plane** (Master) and joining **Worker Nodes**.

### Key Initialization Steps

* **SSH Access:** Connect to your instances using `ssh -i key.pem ubuntu@<IP>`.
* **Master Initialization:** Run `kubeadm init` to bootstrap the Control Plane. This generates the certificates and starts core services like the API Server and Scheduler.
* **Joining Workers:** If you lose the initial join string, run `kubeadm token create --print-join-command` on the Master to get a new one.
* **Networking (CNI):** A CNI plugin (like Weave or Calico) is mandatory for Pod-to-Pod communication. Install it using `kubectl apply -f <CNI_URL>`.
* **Verification:** Use `kubectl get nodes` to ensure all nodes are in the **Ready** state.


## 2. Pod Basics

A Pod is the smallest unit in Kubernetes, representing a group of one or more containers that share storage and network resources.

### Managing Pods

* **Ad-hoc Creation:** `kubectl run <name> --image=nginx --restart=Never`.
* **Inspection:** `kubectl get po -o wide` shows which node the Pod is running on and its internal IP.
* **Deletion:** `kubectl delete po <name>`. To clear everything in a namespace, use `kubectl delete po --all`.
* **Namespaces:** Logical partitions for your cluster. Use `kubectl create ns <name>` and target it with `-n <name>`.


## 3. The ReplicaSet (RS)

A ReplicaSet ensures that a specific number of Pod replicas are running at all times. If a Pod crashes or a node fails, the ReplicaSet creates a replacement.

### ReplicaSet Mechanics

* **Desired State:** Defined in `spec.replicas`.
* **Reconciliation Loop:** The controller constantly compares the "Actual State" (running pods) with the "Desired State."
* **Labels and Selectors:** The RS identifies "its" Pods using `matchLabels`. The Pod template inside the RS must have labels that match this selector.
* **Self-Healing:** * If a container crashes, the **Kubelet** restarts it locally.
* If a Pod is deleted or a Node dies, the **ReplicaSet Controller** creates a new Pod on a healthy node.


## 4. Control Plane Flow

When you run `kubectl apply -f rs.yaml`, the following happens:

1. **API Server:** Authenticates the request and saves the RS object to **etcd**.
2. **RS Controller:** Sees the object in etcd, realizes Pods are missing, and requests the API Server to create Pod objects.
3. **Scheduler:** Detects Pods with no assigned node and selects the best Worker Node based on resources.
4. **Kubelet:** On the target Worker Node, Kubelet sees the assignment, pulls the image via the **Container Runtime** (CRI-O/containerd), and starts the container.


## 5. Troubleshooting Cheat Sheet

| Issue | Common Reason | Diagnostic Command |
| --- | --- | --- |
| **Pending** | Insufficient CPU/RAM or taints. | `kubectl describe pod <name>` |
| **ImagePullBackOff** | Wrong image name or private registry issues. | `kubectl describe pod <name>` |
| **CrashLoopBackOff** | App is crashing internally. | `kubectl logs <name>` |
| **Node NotReady** | Kubelet is down or CNI is not installed. | `systemctl status kubelet` |


## 6. Important Commands

* **Apply Manifest:** `kubectl apply -f file.yaml`
* **Check Status:** `kubectl get rs` or `kubectl get pods`
* **Detailed View:** `kubectl describe rs <name>`
* **Owner Check:** `kubectl get pod <name> -o yaml` (Look for `ownerReferences`).
* **Cascade Delete:** `kubectl delete rs <name>` deletes the RS and its pods. Use `--cascade=orphan` to keep pods running.