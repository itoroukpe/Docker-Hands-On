Great! Here's a **simple 3-tier application setup** using:

* **Frontend**: HTML, CSS, JavaScript (fetch API)
* **Backend**: Spring Boot (Java)
* **Database**: MongoDB

We'll connect all of these using **Docker and Docker Compose**, ensuring they run as isolated but networked services.

---

## 📦 Folder Structure

```
3-tier-app/
├── backend/
│   └── (Spring Boot app with REST API)
├── frontend/
│   ├── index.html
│   ├── styles.css
│   └── app.js
├── docker-compose.yml
└── README.md
```

---

## 🧱 1. `frontend/index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>3-Tier App</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>Simple Form</h1>
  <form id="dataForm">
    <input type="text" id="name" placeholder="Enter Name" required />
    <button type="submit">Submit</button>
  </form>
  <div id="result"></div>

  <script src="app.js"></script>
</body>
</html>
```

---

## 🎨 2. `frontend/styles.css`

```css
body {
  font-family: Arial, sans-serif;
  padding: 20px;
}
input, button {
  padding: 10px;
  margin: 5px;
}
```

---

## 🧠 3. `frontend/app.js`

```javascript
document.getElementById('dataForm').addEventListener('submit', async function (e) {
  e.preventDefault();
  const name = document.getElementById('name').value;

  const response = await fetch('http://localhost:8080/api/data', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name })
  });

  const result = await response.json();
  document.getElementById('result').textContent = `Saved: ${result.name}`;
});
```

---

## ⚙️ 4. Sample Spring Boot Controller (Backend)

```java
@RestController
@RequestMapping("/api/data")
@CrossOrigin(origins = "*") // for local dev
public class DataController {

    @Autowired
    private DataRepository repo;

    @PostMapping
    public Data saveData(@RequestBody Data data) {
        return repo.save(data);
    }

    @GetMapping
    public List<Data> getAllData() {
        return repo.findAll();
    }
}
```

---

## 📁 5. Sample MongoDB Entity & Repo

```java
@Document
public class Data {
    @Id
    private String id;
    private String name;
}
```

```java
public interface DataRepository extends MongoRepository<Data, String> {}
```

---

## 🐳 6. `docker-compose.yml`

```yaml
version: '3.8'

services:
  mongo:
    image: mongo
    container_name: mongodb
    environment:
      MONGO_INITDB_ROOT_USERNAME: devdb
      MONGO_INITDB_ROOT_PASSWORD: devdb1234
    volumes:
      - db_data:/data/db
    networks:
      - appnet

  backend:
    build: ./backend
    container_name: springapp
    ports:
      - "8080:8080"
    environment:
      - MONGO_DB_HOSTNAME=mongodb
      - MONGO_DB_USERNAME=devdb
      - MONGO_DB_PASSWORD=devdb1234
    depends_on:
      - mongo
    networks:
      - appnet

  frontend:
    image: nginx:alpine
    container_name: frontend
    ports:
      - "80:80"
    volumes:
      - ./frontend:/usr/share/nginx/html
    networks:
      - appnet

volumes:
  db_data:

networks:
  appnet:
```

---

## ✅ How to Run

1. Create backend Spring Boot project and package it.
2. Place frontend files in `frontend/`.
3. Run:

```bash
docker-compose up --build
```

4. Open: [http://localhost](http://localhost) to test the app.

---


