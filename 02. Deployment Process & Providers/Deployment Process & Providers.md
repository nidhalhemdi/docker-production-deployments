# **Deployment Process & Providers**

---

## 🚀 **1. Deployment Scenarios**

Docker projects can vary:

* **Single container** (simple Node.js app)
* **Multiple containers** (frontend, backend, DB)
* **Deploying on one server or multiple servers**

We will start with the **simplest case**:
✔ Single Node.js app
✔ One image → One container
✔ One target server

---

## 🧭 **2. Basic Deployment Workflow (Manual Deployment)**

This is the standard “self-managed server” deployment flow:

### **Deployment Steps**

1. **Create a remote server** (in the cloud)
2. **Connect to it (SSH)**
   Example:

   ```bash
   ssh -i your-key.pem ubuntu@your-server-ip
   ```
3. **Install Docker** on the remote server
4. **Push your image** from your local machine → **Docker Hub**

   ```bash
   docker push your-username/your-image:tag
   ```
5. **Pull the image** on the remote server

   ```bash
   docker pull your-username/your-image:tag
   ```
6. **Run the container** on the remote server

   ```bash
   docker run -d -p 80:80 your-username/your-image:tag
   ```
7. **Expose ports** so users can access the app through the browser.

This is the foundational deployment pattern.

---

## 🌐 **3. Hosting Providers**

You can deploy Docker containers on:

* Many small hosting companies (hundreds online)
* Any cloud provider that supports Docker

But the **three biggest providers** are:

1. **AWS (Amazon Web Services)**
2. **Microsoft Azure**
3. **Google Cloud Platform (GCP)**

These companies offer **complete cloud ecosystems** (not just hosting):

* Container hosting
* Databases
* Storage
* Machine learning
* Networking tools
  … and much more.

---

## ☁️ **4. Why AWS is Used Here**

* AWS is the **largest** cloud provider.
* Has multiple Docker-related services.
* Good for demonstrating different deployment methods.


⚠️ **Note:**
AWS (and all major cloud providers) **require a credit card**.

---

## 💸 **5. AWS Free Tier (Important)**

AWS offers a **free tier**, especially useful for learning:

* **EC2 remote server**:
  ✔ 750 hours/month for 12 months (enough for running 1 server continuously)

Always check the **AWS pricing page** to avoid surprises.

---

## 🎯 **Key Takeaways**

* Discover the fundamental deployment workflow using **manual server setup**.
* Understand how to push → pull → run containers on a remote host.
* AWS is used for examples, but the concepts apply everywhere.
* Free tier makes AWS great for initial experiments.
* This is the starting point before exploring more advanced/managed deployment options.
