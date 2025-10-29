## 1. What is DevOps?
DevOps isn't a tool you install; it's a **way of working**. It merges **Development** (the builders) with **Operations** (the maintainers) to stop the "it worked on my machine" excuses.

* **The Goal:** Ship updates faster, automate boring manual tasks, and make systems that don't crash when things get busy.
* **The Vibe:** Constant feedback and constant improvement.


## 2. The Software Journey (SDLC)
Every app follows this lifecycle from "idea" to "reality":
1.  **Planning:** Figuring out what the business actually needs.
2.  **Architecture:** Drawing the blueprint of how it will work.
3.  **Coding:** The actual building/programming phase.
4.  **Testing:** Trying to break the app before the users do.
5.  **Release:** Pushing the app to the live servers.
6.  **Monitoring:** Watching the app to fix bugs or performance lags.


## 3. The DevOps Lifecycle & Tools
To keep the loop moving, we use specific tools at every stage:

| Phase | What Happens | Popular Tools |
| :--- | :--- | :--- |
| **Plan/Code** | Organizing tasks and writing code. | Jira, Git, GitHub |
| **Build/Test** | Compiling the app and running auto-tests. | Maven, Jenkins, Selenium |
| **Deploy** | Putting the app into "containers." | Docker, Kubernetes |
| **Monitor** | Watching the "heartbeat" of the app. | Prometheus, Grafana |


## 4. DevSecOps (Security by Default)
Instead of checking for hacks at the very end, we **"Shift-Left"**—meaning we check for security holes while we are still coding.

* **SAST:** Scans your raw code for "dumb" mistakes before it's even run.
* **DAST:** Attacks the running app to see if it has open doors.
* **SCA:** Scans your "ingredients" (third-party libraries) to make sure they aren't poisoned/hacked.
* **Linting:** A "grammar check" for code to keep it clean and standard.


## 5. Infrastructure as Code (IaC)
In the old days, you’d manually set up a server. Now, you write a **script** that builds the server for you.
* **Benefits:** It’s fast, consistent, and if you accidentally delete your server, you just run the script to get it back in seconds.
* **Tools:** Terraform (for cloud setup), Ansible (for configuring the inside of the server).


## 6. AWS VPC: Your Private Corner of the Cloud
A **VPC** is like having your own private, fenced-off data center inside Amazon’s cloud.

* **Subnets:** Small sections inside your fence.
    * **Public Subnet:** Where your website lives (it can see the internet).
    * **Private Subnet:** Where your secret data lives (it’s hidden from the web).
* **Gateways:**
    * **Internet Gateway (IGW):** The "Front Door" for your public servers.
    * **NAT Gateway:** A "One-Way Mirror"—let’s your private servers go out to download updates, but won't let strangers in.
* **Firewalls:**
    * **Security Groups:** Protects a single server.
    * **NACLs:** Protects the entire subnet "neighborhood."


## 7. Smart Deployment Strategies
How to update your app without the "Site Down for Maintenance" screen:

1.  **Blue-Green:** You have the old version (Blue) and the new version (Green) running at the same time. Once you're sure Green works, you point all users to it.
2.  **Canary:** You give the new update to 1% of users. If they don't complain, you roll it out to everyone else.
3.  **Rolling:** You update servers one at a time, so you never lose your total capacity.