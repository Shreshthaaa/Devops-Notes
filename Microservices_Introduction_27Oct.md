## 1. Architecture Styles

### Monolithic vs. Microservices
* **Monolithic:** The "All-in-One" approach. The frontend, backend, and database are one single unit. If you want to change the color of a button, you have to redeploy the whole giant system.
* **Microservices:** The "Modular" approach. The app is broken into small, independent services (e.g., User Service, Payment Service). Each runs on its own and talks to others via **APIs**.




## 2. Traffic & Networking

### DNS (The Internet's Phonebook)
Computers use numbers (**IP Addresses**) to find each other. Humans use names (**Domain Names**). 
* **Role:** DNS translates `google.com` into `142.250.183.68`.
* **Benefit:** You don't have to memorize random numbers, and companies can move servers without changing their web address.

### Load Balancer (The Traffic Cop)
Once DNS finds the right address, a **Load Balancer** manages the incoming "crowd" by spreading requests across multiple servers.
* **Path-based:** Sends `/users` traffic to the User Service and `/payments` to the Payment Service.
* **Instance-based:** Evenly distributes work among several copies of the same service to prevent crashes.


## 3. Security: Who vs. What
| Concept | Simple Definition | Example |
| :--- | :--- | :--- |
| **Authentication (AuthN)** | Confirms **who** you are. | Entering your password to log in. |
| **Authorization (AuthZ)** | Confirms **what** you can do. | An editor can post a blog, but a viewer can only read it. |


## 4. Data & Discovery

### Database per Service
In microservices, **sharing is NOT caring.** * Each service must have its own private database.
* This ensures that if the "Orders" database goes down, the "Catalog" service still works perfectly.

### Service Discovery
In a cloud environment, servers (IP addresses) are constantly changing.
* **The Registry:** Like a live directory. When a service starts, it registers its current "phone number" (IP/Port).
* **The Lookup:** When Service A needs Service B, it asks the registry: "Where is Service B right now?"


## 5. Communication Patterns

### Synchronous (Wait for it...)
Like a **Phone Call**. You send a request (REST/gRPC) and wait on the line for the response.
* **Best for:** Things that need an immediate answer (e.g., "Is this password correct?").

### Asynchronous (Set and forget)
Like a **Text Message**. You send a message to a **Message Broker** (Kafka/RabbitMQ) and move on to your next task.
* **Best for:** Tasks that take time (e.g., sending an email, processing a video).



## 6. Event Sinking (The Black Box)
This is the practice of collecting "breadcrumbs" from every service. It tracks:
1. Which service handled the request?
2. Did it succeed or fail?
3. How long did it take?
* **Tools:** ELK Stack (Elasticsearch, Logstash, Kibana) or Prometheus/Grafana.


## 7. The 12-Factor App (Golden Rules)
Developed by Heroku, these are 12 rules for building "cloud-native" apps that are easy to scale and maintain.

1.  **Codebase:** One Git repo = many deployments.
2.  **Dependencies:** Explicitly list every library you use.
3.  **Config:** Store passwords/secrets in environment variables, not code.
4.  **Backing Services:** Treat databases like attached plugins.
5.  **Build, Release, Run:** Strictly separate the stages of deployment.
6.  **Processes:** The app should be stateless (don't save files locally).
7.  **Port Binding:** The app should be its own web server.
8.  **Concurrency:** Scale out by adding more copies of the app.
9.  **Disposability:** Start fast and shut down cleanly.
10. **Dev/Prod Parity:** Keep your local setup as close to the server as possible.
11. **Logs:** Treat logs as a continuous stream of events.
12. **Admin Processes:** Run maintenance tasks as one-off scripts.