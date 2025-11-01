## CI/CD: The Automation Engine

CI/CD is a method to automate the building, testing, and delivery of software. It replaces manual handovers with a streamlined pipeline to make releases faster and more frequent.

### Continuous Integration (CI)

CI is the process where developers merge their code into a central repository multiple times a day. Each merge triggers an automated build and test sequence.

* **Goal:** Catch bugs immediately after code is written.
* **Standard CI Workflow:**
1. Code is pushed to a repository like GitHub.
2. The CI server downloads code and installs necessary tools.
3. The code is compiled and packaged.
4. Automated checks (Linting) ensure the code is clean.
5. Unit tests check small pieces of logic.
6. Security scans (SAST/SCA) check for vulnerabilities.
7. The application is packaged into a Docker image.
8. The final package is stored in a repository like JFrog or AWS ECR.


### Continuous Delivery vs. Continuous Deployment

* **Continuous Delivery:** The code is always in a "ready-to-ship" state. It is automatically tested and pushed to a staging area, but a human must click a button to push it to the live production environment.
* **Continuous Deployment:** There is no "manual button." If the code passes every single test in the pipeline, it goes live to customers automatically.


## Testing and Mocking

### Testing Categories

* **Unit Testing:** Testing a single function or method in total isolation.
* **Integration Testing:** Checking if two or more modules work together correctly.
* **System Testing:** Testing the entire application from the user's perspective.
* **Performance Testing:** Stress-testing the app to see how many users it can handle before crashing.
* **DAST:** Testing the security of the application while it is actually running.

### Mocking Explained

When testing a specific function that relies on an external database or another service, we use a **Mock**. A Mock is a "fake" version of that dependency that gives a pre-defined answer. This allows you to test your code without needing the entire system to be online.


## Linux Essentials

Linux is the backbone of the internet, powering the vast majority of production servers worldwide.

### The Kernel

The Kernel is the core of the OS. It sits between the hardware (CPU, RAM) and the software (Applications). Its jobs include:

* Deciding which program gets to use the CPU.
* Managing memory and storage.
* Controlling hardware devices like keyboards and network cards.

### Core Components

* **Shell:** The text-based interface where you type commands (e.g., Bash).
* **File System:** The tree-like structure starting at the root `/`.
* **Processes:** Any program currently running on the system.


## AWS Networking: IP Address Types

AWS uses three specific ways to identify your servers (EC2 instances):

* **Private IP:** The internal address used for servers to talk to each other inside the same network. It does not change if the server reboots.
* **Public IP:** The address used to reach the server from the internet. This address is lost and replaced with a new one every time the server is stopped and started.
* **Elastic IP (EIP):** A fixed, permanent public address. You can move an Elastic IP from one server to another, making it ideal for keeping a consistent entry point for users.