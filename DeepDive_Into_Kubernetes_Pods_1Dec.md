## 1. The Anatomy of a Pod

A Pod is more than just a container; it is an **execution environment**.

* **The Pause Container (Infra Sandbox):** Every Pod starts with a "hidden" container called `pause`. Its only job is to hold the Network Namespace (IP and port space) so that if your app container crashes and restarts, the Pod's IP address doesn't change.
* **Shared Localhost:** Because they share the same network stack, a helper (sidecar) container can talk to the main app container using `localhost:8080`.


## 2. The Detailed Pod Creation Flow

Your trace of the API calls is spot on. Here is a simplified visual mapping of that "Handshake":

1. **kubectl** → Sends YAML to **API Server**.
2. **API Server** → Validates and saves to **etcd**.
3. **Scheduler** → Watches **etcd**, picks a Node, and tells **API Server**.
4. **Kubelet** → Watches **API Server**, sees its name on the Pod, and starts the work.
5. **CRI (Containerd)** → Pulls the image and starts the process.
6. **CNI (Weave/Calico)** → Assigns the IP address.


## 3. Troubleshooting "Common Pod Failures"

| Status | What it usually means | Next Step |
| --- | --- | --- |
| **ImagePullBackOff** | Wrong image name or private registry. | `kubectl describe pod` to see the URL. |
| **CrashLoopBackOff** | App started but crashed (code error). | `kubectl logs <pod> --previous` |
| **Pending** | No node has enough CPU/RAM. | `kubectl describe pod` to see "Scheduling failed". |
| **ContainerCreating** | Network (CNI) or Storage (CSI) is stuck. | Check `systemctl status kubelet` on the worker. |


## 4. Networking: The CNI Layer

In a `kubeadm` cluster, nodes stay in a `NotReady` state until the CNI is installed.

* **The CNI's Job:** It creates a "virtual bridge" on every node.
* **Pod-to-Pod:** Every Pod gets a unique IP that is reachable from any other Pod in the cluster, regardless of which node it is on.


## 5. Tips

* **The "Wide" View:** Always use `kubectl get pods -o wide`. It tells you exactly **which worker node** your pod landed on, which is essential when you have 2+ workers.
* **Dry Run:** Before creating a pod, you can generate the YAML without applying it:
`kubectl run mypod --image=nginx --dry-run=client -o yaml > pod.yaml`
* **Namespace Shortcut:** If you are tired of typing `-n scaler`, you can change your "context" permanently:
`kubectl config set-context --current --namespace=scaler`