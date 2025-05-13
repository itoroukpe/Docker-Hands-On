**step-by-step, hands-on practical guide** to setting up a Spring Boot application with MongoDB using Docker networking and volumes for data persistence:

---

### **🛠️ 1. Create Docker Bridge Network**

```bash
docker network create -d bridge rondustech
```

This creates a custom network so that containers can talk to each other by name.

---

### **📦 2. Create MongoDB Container Without Volume**

```bash
docker run -d \
  --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb1234 \
  --network rondustech \
  mongo
```

MongoDB is running with default internal storage. Deleting this container will lose data.

---

### **🚀 3. Run Spring Boot App in Same Network**

```bash
docker run --name springapp -d -p 8080:8080 \
  --network rondustech \
  -e MONGO_DB_USERNAME=devdb \
  -e MONGO_DB_PASSWORD=devdb1234 \
  -e MONGO_DB_HOSTNAME=mongodb \
  rondustech/spring-boot-mongo
```

Spring app communicates with MongoDB using container name `mongodb`.

---

### **🔍 4. Insert Data and Validate**

* Open browser: [http://localhost:8080](http://localhost:8080)
* Use the app to insert data (e.g., via a form or API).
* Then, **stop and remove** MongoDB:

  ```bash
  docker rm -f mongodb
  ```
* Recreate it **without a volume** and verify data is lost.

---

### **📁 5. Using Bind Mount (for testing)**

```bash
mkdir $(pwd)/data

docker run -d \
  --name mongodb \
  -v $(pwd)/data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb1234 \
  --network rondustech \
  mongo
```

This allows you to view/edit the database files directly from your local machine.

---

### **🧱 6. Create Docker Volume (recommended)**

```bash
docker volume create DBbackup
docker volume ls
```

Docker manages the volume lifecycle and location.

---

### **🧹 7. Clean Up Old Containers**

```bash
docker rm -f springapp mongodb
```

---

### **💾 8. Recreate MongoDB Container with Docker Volume**

```bash
docker run -d \
  --name mongodb \
  -v DBbackup:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb1234 \
  --network rondustech \
  mongo
```

✅ This ensures MongoDB persists data even if the container is deleted and recreated.

---

### **🚀 9. Recreate Spring Boot App**

```bash
docker run -d -p 8081:8080 \
  --name springapp \
  -e MONGO_DB_HOSTNAME=mongodb \
  -e MONGO_DB_USERNAME=devdb \
  -e MONGO_DB_PASSWORD=devdb1234 \
  --network rondustech \
  rondustech/spring-boot-mongo
```

---

### **🌐 10. Access App and Test Data Persistence**

1. Go to [http://localhost:8081](http://localhost:8081)
2. Add data again.
3. Run:

   ```bash
   docker rm -f mongodb
   ```
4. Recreate MongoDB with volume:

   ```bash
   docker run -d \
     --name mongodb \
     -v DBbackup:/data/db \
     -e MONGO_INITDB_ROOT_USERNAME=devdb \
     -e MONGO_INITDB_ROOT_PASSWORD=devdb1234 \
     --network rondustech \
     mongo
   ```
5. ✅ **Check that your data is still available**.

---


