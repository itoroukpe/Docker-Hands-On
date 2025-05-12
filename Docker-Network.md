Here's a **hands-on DevOps Masterclass** lab for **RondusTech** focused on **Docker containerization and networking**, based on the image `rondustech/maven-web-app` from Docker Hub.

---

## 🛠️ Hands-On Lab: Create a Container from a Docker Image and Network It

### 🔧 Prerequisites

Ensure you have Docker installed on your system:

```bash
docker --version
```

---

## Step 1: 📥 Pull the Docker Image

```bash
docker pull rondustech/maven-web-app
```

This command fetches the image from Docker Hub.

---

## Step 2: 🚀 Run the Container

```bash
docker run -d --name maven-web-container -p 8080:8080 rondustech/maven-web-app
```

### Explanation:

* `-d` = Detached mode
* `--name` = Names your container
* `-p 8080:8080` = Maps host port 8080 to container port 8080

Check if it's running:

```bash
docker ps
```

Open the app:

```
http://localhost:8080
```

---

## Step 3: 🌐 Create a Docker Network

```bash
docker network create --driver bridge rondus-net
```

### Explanation:

* `--driver bridge`: Creates an isolated network using the default bridge driver
* `rondus-net`: Custom network name

View your network:

```bash
docker network ls
```

---

## Step 4: 🔗 Connect the Container to the Network

First, stop the container if running:

```bash
docker stop maven-web-container
docker rm maven-web-container
```

Now, run it again attached to the custom network:

```bash
docker run -d --name maven-web-container --network rondus-net -p 8080:8080 rondustech/maven-web-app
```

---

## Step 5: 🧪 Verify Network Connection

You can inspect the container’s network setup:

```bash
docker inspect maven-web-container
```

Look for `"Networks": { "rondus-net": ... }` in the output.

---

## Optional Bonus: 🗣️ Ping Between Containers

If you run another container on the same network:

```bash
docker run -it --name test-container --network rondus-net alpine sh
```

Then from inside the container:

```sh
ping maven-web-container
```

This verifies that containers on the same custom network can communicate by container name.

---


