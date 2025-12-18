# 🚀 Understanding Multi-Stage Build Targets in Docker

This explains a useful feature of multi-stage Docker builds:
👉 **You can choose to build only part of the Dockerfile**, instead of building all stages.

---

## 🧱 1. What is a Multi-Stage Dockerfile?

A multi-stage Dockerfile has several `FROM` instructions, each defining a “stage”.

Example (your React Dockerfile):

```Dockerfile
FROM node:24 AS build
# build frontend

FROM nginx:alpine
# serve frontend build
```

* **Stage 1** → builds React app
* **Stage 2** → uses Nginx to serve the built files

Normally, Docker builds **all stages**, from top to bottom.

---

## 🎯 2. What is `--target`?

Sometimes you want to build **only one specific stage** instead of the full final image.

Docker allows this with:

```
docker build --target <stage-name> .
```

For your React production Dockerfile, you could run:

```
docker build -f Dockerfile.prod --target build .
```

This means:

* Docker stops **after finishing the “build” stage**
* It **does not** continue to the final Nginx stage
* The resulting image only contains the files from the selected stage

---

## 🎁 3. When is this useful?

In larger projects, `--target` becomes extremely powerful:

### 🧪 Example: Testing stage

You could have stages like:

* `test`
* `build`
* `production`

Then:

```
docker build --target test .
```

Lets you:

✔ Build **only the test stage**
✔ Run tests quickly without building extra layers
✔ Save time in CI pipelines

Or:

```
docker build --target build .
```

Lets you:

✔ Build your app but **not** package it for production

---

## 📌 4. Key Takeaways

* Multi-stage Dockerfiles allow many stages inside one file.
* You *don’t* always need to build the final image.
* `--target` lets you stop the build after any stage.
* Useful for:

  * Running tests
  * Building only parts of a pipeline
  * Faster development workflows
  * CI/CD setups
