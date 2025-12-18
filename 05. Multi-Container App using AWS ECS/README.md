# **Preparing a Multi-Container App (Multi-Container Deployment on AWS ECS)**

This section explains how to prepare a **multi-container application** for deployment to **AWS ECS**, using a backend Node.js API + MongoDB database as the example.

---

## **1. Multi-Container Deployment Goal**

Previously, you deployed a **single container** to ECS.
Now you will deploy **two containers**:

* **Backend API container** (Node.js)
* **MongoDB database container**

The **frontend is intentionally missing** in this example.

---

## **2. Important: Docker Compose Is NOT Used for Cloud Deployment**

Docker Compose is great for:

* Local multi-container setups
* Local container networking
* Easily running backend + DB + frontend together

But **it is not suited for cloud deployments**, because:

* Hosting providers (AWS, Azure, etc.) need extra settings (CPU, RAM, scaling…)
* Compose is not designed to describe cloud infrastructure
* In the cloud, containers may run on **different machines**, unlike local Compose networks

So the compose file is only used as **inspiration** for deployment configuration.

---

## **3. Clean Up Old ECS Resources**

Before deploying the new multi-container app:

* Delete any existing ECS services
* Delete the ECS cluster
  (Deletion may take a few minutes.)

---

## **4. Prepare the Backend Container**

You need to:

1. **Build** the backend image
2. **Push** it to Docker Hub
3. Later use it inside AWS ECS

Example:

```bash
docker build -t goals-node ./backend
docker tag goals-node username/goals-node
docker push username/goals-node
```

---

## **5. Critical Point: Container Networking Works Differently in ECS**

### **Local behavior with Docker Compose**

Locally, the backend connects to MongoDB via:

```
mongodb://mongodb:27017
```

where **“mongodb”** is the container name.
Docker Compose creates an internal network and resolves container names automatically.

### **AWS ECS behavior**

This **will NOT work** in ECS because:

* Containers may run on different machines
  (unless they are in the same *task*)
* ECS does **NOT** create a Docker network like Compose does

### **Exception: Same ECS Task**

If both backend and database run inside the **same ECS Task**:

* They run on the **same machine**
* They can reach each other via **localhost**

Thus, in production:

```
mongodb://localhost:27017
```

But locally, you still need:

```
mongodb://mongodb:27017
```

---

## **6. Solution: Use Environment Variables**

To avoid changing code for each environment:

In code:

```js
const MONGO_URL = process.env.MONGODB_URL;
```

Local `.env`:

```
MONGODB_URL=mongodb
```

ECS environment variable:

```
MONGODB_URL=localhost
```

This keeps development and production consistent.

---

## **7. Push the Backend Image to Docker Hub**

After adjusting code:

1. Build the backend image
2. Tag it for Docker Hub
3. Push it

Only the **image** is pushed.
**Environment variables are NOT included** in the image — they are applied when running the container.

This is the same behavior as with Docker Compose:
env files are only read at container runtime.

---

## **Summary**

This section of the project prepares you for multi-container deployment:

* Docker Compose is not used for cloud deployment.
* ECS tasks allow multiple containers to share **localhost** networking.
* Backend code must be adjusted to use **environment variables** for DB URLs.
* Build and push your backend image to Docker Hub.
* MongoDB will also be deployed inside the same ECS task to make networking work.
