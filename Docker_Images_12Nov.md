# Understanding Virtualization and Docker

## 1. Bare Metal Architecture

Bare metal is the traditional way of running applications where the software is installed directly on the physical hardware.

* **Layers:** Hardware (CPU/RAM) → Kernel → Libraries/Binaries → Application.
* **The Limit:** You can only run one Operating System at a time. While it offers full performance, it is difficult to scale or divide resources among different tasks.


## 2. Hypervisor-Based Virtualization

A Hypervisor is a layer of software that lets you carve up one physical computer into several "Virtual Machines" (VMs).

* **How it works:** The Hypervisor divides hardware resources (vCPU, vRAM) so that each VM thinks it is a real, independent computer.
* **Type 1 (Bare Metal):** Installed directly on the hardware (e.g., VMware ESXi).
* **Type 2 (Hosted):** Installed as an app on an existing OS (e.g., VirtualBox).

### Why VMs are "Heavy"

Every time you start a VM, it performs a **Full Boot Sequence**:

1. **BIOS:** Checks hardware.
2. **MBR:** Locates the OS.
3. **Bootloader:** Loads the Kernel.
4. **Kernel:** Starts system engines.
5. **Init:** Launches OS services.

Because every VM has its own full OS, they consume a lot of RAM and take minutes to start.


## 3. The Docker Revolution

Docker changes the game by eliminating the need for a guest OS.

* **The Secret:** Docker containers **share the Host OS Kernel**. They do not have a BIOS, MBR, or their own Kernel.
* **The Result:** Containers start in milliseconds because they only start the application process, not a whole operating system.

| Feature | Virtual Machines | Docker Containers |
| --- | --- | --- |
| **Guest OS** | Full OS for every VM | No Guest OS (uses Host Kernel) |
| **Startup** | Minutes (Boot sequence) | Milliseconds (Process start) |
| **Weight** | Heavy (GBs) | Lightweight (MBs) |
| **Performance** | Slight overhead | Near native speed |


## 4. Docker Architecture and Flow

The Docker ecosystem consists of four main parts:

1. **Docker Client:** The CLI where you type commands (e.g., `docker run`).
2. **Docker Daemon (dockerd):** The brain that builds and manages containers on the host.
3. **Docker Host:** The server where images and containers live.
4. **Docker Registry:** A cloud library (Docker Hub) where you pull and push images.

### The "Run" Flow

When you type `docker run <image>`:

1. The **Client** talks to the **Daemon**.
2. The **Daemon** checks if the image is local. If not, it pulls it from **Docker Hub**.
3. The **Daemon** creates an isolated environment (filesystem, network, process tree).
4. The application starts instantly using the shared host kernel.


## 5. Essential Docker Commands

### Managing Containers

* **List running:** `docker ps`
* **List all (including stopped):** `docker ps -a`
* **Run in background:** `docker run -d <image>`
* **Inspect details (IP/Network):** `docker container inspect <id>`
* **Enter a running container:** `docker exec -it <id> bash`

### Creating and Sharing Images

* **Login to Hub:** `docker login`
* **Save changes to a new image:** `docker commit -m "notes" <id> <username/image>`
* **Upload to Hub:** `docker push <username/image>`
* **Delete an image:** `docker rmi <image-id>`