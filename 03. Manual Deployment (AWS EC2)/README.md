# **Getting Started With an Example**

---

## 🎯 **Goal of This Example**

Deploy a **very simple Node.js app** (no database) to an **AWS EC2 instance** using Docker.

This example teaches the *full manual deployment flow*:

1. Create a remote server
2. Configure its security
3. Connect to it
4. Install Docker
5. Pull & run your container

---

## 🖥️ **1. What Is AWS EC2?**

* EC2 = A service by AWS that gives you **remote virtual machines** (cloud servers).
* You control them just like real computers.
* You can install anything on them → in this case: **Docker**.

We will:

* Launch an EC2 instance
* Use a **VPC** (Virtual Private Cloud)
* Use a **Security Group** (firewall rules)
* Connect via **SSH**

---

## 🛠️ **2. Deployment Steps Overview**

### ✔ Step 1: Create EC2 Instance

* Start a remote machine in AWS.
* During creation, AWS forces you to also create:

  * **VPC** → your private network
  * **Security Group** → controls access (which ports are open)

### ✔ Step 2: Configure Security Group

To make the app publicly reachable:

* Allow incoming traffic (HTTP) on port **80**
* Allow SSH (port **22**) so you can connect

### ✔ Step 3: Connect via SSH

You will log into the EC2 machine using terminal commands.

### ✔ Step 4: Install Docker on the EC2 server

You run installation commands **inside the remote terminal**.

### ✔ Step 5: Push & Pull Image

* Push your image from your local machine → Docker Hub
* Pull it on the EC2 machine
* Run the container there

---

## 📦 **3. The Example Application**

The provided app:

* Node.js server serving a single HTML file (`welcome.html`)
* Listens on port **80**
* Comes with a simple **Dockerfile** (using `node:alpine` for small size)

---

## 👨‍💻 **4. Local Docker Workflow (Before Deployment)**

### **Build the image**

Run inside the project folder containing `Dockerfile`:

```bash
docker build . -t node-dep-example
```

### **Run the container locally**

```bash
docker run -d --rm \
  --name node-dep \
  -p 80:80 \
  node-dep-example
```

Explanation:

* `-d` = detached
* `--rm` = auto-remove when stopped
* `-p 80:80` = expose port 80 to your machine
* No volumes or networks needed

### **Test locally**

Open in your browser:

```
http://localhost
```

You should see the **demo welcome screen**.

---

## 🎯 **Key Takeaways**

* You start with a simple app to learn deployment basics.
* AWS EC2 provides a real remote server → you control everything.
* Security Groups define who can access your app (open ports).
* You build + push locally, then pull + run on EC2.
* The local run workflow stays identical to earlier sections.
