# Persistent Storage: Docker Volumes

## 1. The Ephemeral Problem

By default, containers are "ephemeral," meaning they are temporary.

* **The Writable Layer:** When you run a container, Docker adds a temporary layer where files are written.
* **The Risk:** If you stop and start a container, your data is safe. But the moment you run `docker rm` to delete the container, that writable layer is destroyed forever.


## 2. What is a Docker Volume?

A volume is a dedicated folder on your host machine (usually at `/var/lib/docker/volumes`) that is "plugged into" your container. Because the volume lives outside the container, the data stays safe even if the container is deleted.


## 3. The Four Types of Storage

### Named Volumes (Recommended for Production)

You give the volume a specific name. Docker manages where it is stored.

* **Use Case:** Databases (MySQL, Postgres) and persistent app data.
* **Command:** `docker run -v my_data:/app/data nginx`

### Anonymous Volumes

Docker creates a volume with a random, long ID instead of a name.

* **Use Case:** Temporary storage that you don't need to track easily.
* **Command:** `docker run -v /app/data nginx`

### Bind Mounts (Recommended for Development)

You map a specific folder from your computer (like your project folder) directly into the container.

* **Use Case:** Real-time coding. You change code on your laptop, and the container sees it instantly.
* **Command:** `docker run -v /Users/shreshtha/project:/app nginx`

### tmpfs Mounts

Data is stored only in the system's **RAM (Memory)**, never on the hard drive.

* **Use Case:** Storing passwords, secrets, or high-speed cache that should vanish when the container stops.
* **Command:** `docker run --tmpfs /app/cache nginx`


## 4. Comparison Summary

| Type | Persistent? | Storage Location | Best For |
| --- | --- | --- | --- |
| **Named Volume** | Yes | Docker managed folder | Databases & Production |
| **Anonymous Volume** | Yes | Docker managed folder | Quick temp storage |
| **Bind Mount** | Yes | Any folder you choose | Local Development |
| **tmpfs** | No | System RAM | Secrets & Speed |


## 5. Sharing Data Between Containers

Volumes allow multiple containers to "see" the same files at the same time. If Container A writes a file into a shared named volume, Container B can read it instantly.


## 6. Essential Commands

* **Create a volume:** `docker volume create my_storage`
* **List all volumes:** `docker volume ls`
* **Check volume location:** `docker volume inspect my_storage`
* **Remove a volume:** `docker volume rm my_storage`