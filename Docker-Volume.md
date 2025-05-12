Here’s a **step-by-step hands-on Docker volume practice** tailored for your **RondusTech DevOps Masterclass**, covering all the topics listed:

---

## 🧪 **Docker Volumes Lab Exercises**

---

### 🔹 **1. Introduction to Docker Volumes**

**📘 What is a Docker Volume?**
A volume is a persistent storage mechanism managed by Docker. It’s used to store data outside the container lifecycle.

**🛠️ Create a volume**

```bash
docker volume create rondus-volume
```

**🔍 List volumes**

```bash
docker volume ls
```

**📋 Inspect a volume**

```bash
docker volume inspect rondus-volume
```

---

### 🔹 **2. Persistent Data Storage with Volumes**

This step shows how to store app data outside the container.

**🧪 Example: Start a container with a volume**

```bash
docker run -d \
  --name nginx-volume-demo \
  -v rondus-volume:/usr/share/nginx/html \
  nginx
```

**🌐 Add a test file:**

```bash
docker exec -it nginx-volume-demo bash
echo "Hello from RondusTech" > /usr/share/nginx/html/index.html
exit
```

**🧪 Access the app:**

```
http://localhost:80
```

**📌 Now remove the container and start another using the same volume:**

```bash
docker rm -f nginx-volume-demo

docker run -d \
  --name nginx-volume-demo2 \
  -v rondus-volume:/usr/share/nginx/html \
  -p 80:80 \
  nginx
```

✅ You’ll still see the same data. That’s **persistent storage** in action.

---

### 🔹 **3. Mounting Host Directories as Volumes**

You can also mount your local directories for development or custom data use.

**🧪 Example: Mount current host directory**

```bash
mkdir ~/rondus-data
echo "Local Data from Host" > ~/rondus-data/index.html

docker run -d \
  --name nginx-host-volume \
  -v ~/rondus-data:/usr/share/nginx/html \
  -p 8081:80 \
  nginx
```

**🧪 Test in browser:**

```
http://localhost:8081
```

**⚠️ Note:** Changes to files in `~/rondus-data` will reflect instantly in the container.

---

### 🔹 **4. Docker Volume Drivers and Plugins**

**📘 What are Volume Drivers?**
Drivers let Docker use different backends (e.g., cloud storage, NFS, etc.) to store volumes.

**🔍 List built-in volume drivers**

```bash
docker info | grep "Volume Drivers"
```

**🛠️ Use a custom volume driver (local)**

```bash
docker volume create \
  --driver local \
  --opt type=tmpfs \
  --opt device=tmpfs \
  tmpfs-volume
```

**📋 Check the volume:**

```bash
docker volume inspect tmpfs-volume
```

**🔌 Plugins Example (3rd Party like `rexray/ebs`, `nfs`):**

```bash
docker plugin install vieux/sshfs
```

> For cloud plugins (e.g., AWS EBS or Azure File), you'd usually install plugins or use Docker Swarm/Kubernetes.

---

## ✅ Summary Practice Checklist

| Task                          | Command/Tool                                |
| ----------------------------- | ------------------------------------------- |
| Create Docker volume          | `docker volume create`                      |
| Use volume in container       | `docker run -v`                             |
| Mount host directory          | `docker run -v ~/dir:/container/path`       |
| View & inspect volume         | `docker volume ls`, `docker volume inspect` |
| Explore volume driver options | `docker volume create --driver`             |
| Install volume plugin         | `docker plugin install <name>`              |

---


