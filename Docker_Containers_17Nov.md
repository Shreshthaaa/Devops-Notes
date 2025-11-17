# Understanding Container Internals and Lifecycle

## 1. What Exactly is a Container?

A container is a **sandboxed Linux process**. It is not a mini-computer or an emulator. It is a standard process that has been "tricked" into thinking it is alone on the system.

A container is defined by four things:

* **A Linux Process:** The actual application running (like Nginx).
* **A Temporary Filesystem:** A layered, throw-away set of folders.
* **Namespaces:** These create the "fake world" (private IP, private process list).
* **Cgroups:** These set the "speed limits" (max RAM and CPU usage).


## 2. Hypervisor vs. Containers

The main difference is the **Kernel**.

### Virtual Machines (Heavy)

A Hypervisor creates a VM by simulating hardware. The VM must perform a full boot sequence:

1. **BIOS/MBR:** Checks hardware and finds the OS.
2. **Bootloader:** Loads the kernel.
3. **Kernel:** Starts memory and hardware management.
4. **Init (systemd):** Starts all background OS services.
**Result:** Large resource waste because you are running 5 kernels for 5 apps.

### Containers (Lightweight)

Docker ignores the boot process entirely. It uses the **Host OS Kernel**. When you start a container, it launches the application process immediately.
**Result:** Milliseconds to start and almost zero overhead.


## 3. The Internal "Handshake" (Docker to Process)

When you type `docker run nginx`, a specific chain of command occurs:

1. **Docker Client:** Sends the request to the engine.
2. **containerd:** The high-level manager. It pulls the image and sets up the storage layers.
3. **containerd-shim:** A "babysitter" process. It stays alive so that if Docker restarts, your container doesn't crash.
4. **runc:** The low-level "builder." It talks to the Linux Kernel to create the namespaces and cgroups, then starts the Nginx process.


## 4. How Containers Create a "Fake World"

Containers feel like separate machines because of **Namespaces**. These act as blinders for the process:

* **PID Namespace:** The process thinks its ID is `1`. On your host, it might actually be `9422`.
* **Network Namespace:** The container gets its own IP address and routing table.
* **UTS Namespace:** The container has its own unique hostname.
* **Mount Namespace:** The container can only see the files inside its image, not your host's files.


## 5. Ephemeral Nature & Port Mapping

### Why they are "Ephemeral"

Containers use a **Read-Only** image. When you run one, Docker adds a thin **Writable Layer** on top.

* If you create a file inside a container and then delete the container, that file is **gone forever**.
* To save data, you must use **Volumes** or **Bind Mounts**.

### Port Conflicts

When you run `-p 80:80`, you are "binding" the host's port 80 to the container's port 80.

* You **cannot** run two containers on the same host port.
* Think of it like a house address: two different people (containers) can't have the exact same front door (host port).


## 6. Container Lifecycle Summary

1. **Start:** `runc` creates the isolated environment and starts the main process (PID 1).
2. **Running:** `containerd-shim` monitors the process.
3. **Exit:** If the main process (PID 1) stops, the container dies instantly.
4. **Cleanup:** Docker deletes the writable filesystem and releases the RAM/CPU limits.


## Essential Commands to Remember

* **Check running:** `docker ps`
* **Check all:** `docker ps -a` (shows you containers that crashed or stopped)
* **Start and Map Port:** `docker run -d -p 80:80 nginx`
* **Deep Dive:** `docker inspect <container_id>` (use this to find the container's internal IP)