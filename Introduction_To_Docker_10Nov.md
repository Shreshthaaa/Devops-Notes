# Docker Fundamentals on AWS EC2

## Accessing your AWS Server

To manage Docker on the cloud, you must first connect to your EC2 instance using a secure shell.

1. Locate your private key file (`.pem`) on your computer.
2. Use the terminal to navigate to that folder.
3. Run the SSH command provided by the AWS console, which typically looks like this:
`ssh -i "your-key.pem" ubuntu@your-ec2-public-dns`
4. Once connected, your command prompt will change to show the internal IP of the Ubuntu server.


## The Docker Architecture

Docker works through a conversation between two main parts: the Client and the Daemon.

* **The Docker Client:** This is the tool you type commands into (like `docker run`). It acts as the messenger.
* **The Docker Daemon:** This is a background service (a "daemon" in Linux terms) that does the heavy lifting. It manages images, creates containers, and handles the system's resources.

### Linux Daemons

In Linux, services that run quietly in the background are called daemons. You can check if the Docker engine is running by using the command:
`systemctl status docker`


## Images vs. Containers

Understanding the difference between an image and a container is vital for mastering Docker.

* **Images (The Blueprint):** An image is a read-only template. It contains the OS, the application code, and all necessary libraries. It is like a "Class" in programming.
* **Containers (The Instance):** A container is a living, running instance of an image. It is lightweight, isolated, and has its own writable layer. It is like an "Object."


## Basic Operations

### Running a Container

When you execute `docker run hello-world`, the following happens:

1. The **Client** tells the **Daemon** to run the "hello-world" image.
2. The **Daemon** looks for the image on your server.
3. If it isn't there, it "pulls" (downloads) it from **Docker Hub**.
4. The **Daemon** creates a **brand new container** and starts it.

### Interactive Mode

Sometimes you need to go inside a container to run commands manually.
`docker run -it ubuntu bash`

* **-i (Interactive):** Keeps the connection open.
* **-t (Terminal):** Gives you a screen to type on.
* **bash:** Tells the container to open a specific command-line shell.


## Container Isolation

Containers are famous for being isolated from the host and from each other.

* **Process Isolation:** If you run `ps -ef` inside a container, you will only see the processes belonging to that specific container. It cannot see what the host or other containers are doing.
* **Filesystem Isolation:** Changes made to files inside a container stay inside that container and do not affect the host server.


## Key Takeaways

* Each `docker run` command creates a completely new container instance.
* Images are pulled from a remote registry (Docker Hub) if they are not found locally.
* The Docker Daemon is a background Linux service managed by `systemctl`.
* Containers are much lighter than Virtual Machines because they share the host's Kernel but keep their own files and processes separate.