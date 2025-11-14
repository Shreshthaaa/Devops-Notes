# Docker Image Building & Dockerfile Simplified

## 1. What is a Docker Image?

A Docker image is a **read-only blueprint**. Think of it as a "save file" of a computer system at a specific moment.

* **Layers:** Every time you add a file or install a tool, Docker adds a new "layer."
* **Container:** When you "run" an image, Docker adds a thin, **writable layer** on top where your app can actually perform tasks.


## 2. The Dockerfile

A **Dockerfile** is a text file with instructions on how to assemble the image.

### Core Instructions

* **FROM:** The first line of every Dockerfile. It sets the **base image** (e.g., Ubuntu or Python). It's like choosing the foundation for a house.
* **RUN:** Used during the **Build Phase**. It runs commands *inside* the image to install software or update settings.
* **COPY vs. ADD:** * **COPY:** Your go-to for moving local files into the image.
* **ADD:** Does the same but has "superpowers"—it can download files from URLs or automatically unzip compressed files. Use `COPY` unless you need those extras.


* **WORKDIR:** Sets the "home directory" inside the container so you don't have to type long paths.


## 3. Build Time vs. Run Time

It is critical to know *when* a command happens.

| Phase | Command Type | What it does |
| --- | --- | --- |
| **Image Build** | `FROM`, `RUN`, `COPY`, `WORKDIR` | These are "baked" into the image forever. |
| **Container Run** | `CMD`, `ENTRYPOINT` | These only start working when the container is actually launched. |


## 4. Starting the Application: CMD vs. ENTRYPOINT

### CMD (The Default Argument)

`CMD` defines the command that runs when the container starts. However, it is **flexible**. If a user provides a new command during `docker run`, the `CMD` is completely ignored.

* *Example:* `CMD ["echo", "Hello"]` → Running `docker run myimage "Goodbye"` will output "Goodbye".

### ENTRYPOINT (The Fixed Command)

`ENTRYPOINT` is **not flexible**. It defines the main executable of the container.

* *Example:* `ENTRYPOINT ["python"]` → The container is now a "Python container" and will always try to run Python.

### The Power Combo

You can use them together: `ENTRYPOINT` as the command and `CMD` as the default argument.

```dockerfile
ENTRYPOINT ["echo"]
CMD ["Hello World"]

```

* `docker run myimage` outputs **Hello World**.
* `docker run myimage User` outputs **User**.


## 5. How to Build Your Image

Once your Dockerfile is ready, use the `build` command in your terminal:

```bash
# If your file is named 'Dockerfile'
docker build -t my-app-name .

# If you have a custom name like 'App.Dockerfile'
docker build -t my-app-name -f App.Dockerfile .

```

* **-t:** Stands for "tag" (your image name). Must be lowercase.
* **.** : Represents the **Build Context** (tells Docker to look in the current folder for files).


## 6. Image Maintenance & Inspection

* **Check layers:** `docker history <image-name>` shows every step taken during the build.
* **Details:** `docker inspect <image-name>` shows the hidden metadata (like environment variables).
* **Storage:** On Linux, Docker stores everything internally at `/var/lib/docker`.


## Summary Checklist

1. **FROM** starts the build.
2. **RUN** installs your tools.
3. **COPY** moves your code in.
4. **ENTRYPOINT/CMD** tells the app how to start.
5. **docker build** creates the image.
6. **docker run** brings it to life as a container.