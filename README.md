
# 🚀 Java Spring Boot + React + Docker Compose Deployment

This project contains a **Spring Boot backend**, a **React frontend**, and a **Docker Compose** configuration to run both services together.
The backend connects to an **AWS RDS MySQL** instance.

---

## 📁 Project Structure

```
project-root/
│
├── frontend/         # React app
├── backend/          # Spring Boot app
└── compose/
    ├── docker-compose.yaml
    └── .env
```

---

# 🧩 1. Prerequisites

Make sure your server has:

* Docker
* Docker Compose

Install Docker Compose manually:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.29.2/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

---

# ⚙️ 2. Environment Variables (.env)

Inside `compose/.env`:

```env
# AWS RDS MySQL
SPRING_DATASOURCE_URL=jdbc:mysql://database-1.chiimqu4k79j.us-west-2.rds.amazonaws.com:3306/datastore?createDatabaseIfNotExist=true
SPRING_DATASOURCE_USERNAME=admin
SPRING_DATASOURCE_PASSWORD=12345678

# Ports
BACKEND_PORT=8084
FRONTEND_PORT=8501

# Frontend API call
API_URL=http://54.245.0.104:8084
```

✔ The backend will connect to your RDS database
✔ The frontend will call your backend using `API_URL`

---

# 🐳 3. Docker Compose Configuration

`compose/docker-compose.yaml`:

```yaml
version: "3.8"

services:
  frontend:
    build:
      context: ../frontend
      dockerfile: Dockerfile
    container_name: frontend-app
    environment:
      API_URL: ${API_URL}
    ports:
      - "${FRONTEND_PORT}:8501"
    depends_on:
      - backend

  backend:
    build:
      context: ../backend
      dockerfile: Dockerfile
    container_name: backend-app
    ports:
      - "${BACKEND_PORT}:8084"
    environment:
      SPRING_DATASOURCE_URL: ${SPRING_DATASOURCE_URL}
      SPRING_DATASOURCE_USERNAME: ${SPRING_DATASOURCE_USERNAME}
      SPRING_DATASOURCE_PASSWORD: ${SPRING_DATASOURCE_PASSWORD}
```

---

# ▶️ 4. Running the Application

Go to the `compose` directory:

```bash
cd compose
```

Then build and start containers:

```bash
docker-compose up --build -d
```

Check running containers:

```bash
docker ps
```

---

# 🌐 5. Accessing the Services

| Service  | URL                          |
| -------- | ---------------------------- |
| Frontend | `http://YOUR_SERVER_IP:8501` |
| Backend  | `http://YOUR_SERVER_IP:8084` |

Replace `YOUR_SERVER_IP` with your EC2 / VPS IP.

---

# 🗄️ 6. Connecting to AWS RDS

Your backend connects automatically using:

```
SPRING_DATASOURCE_URL
SPRING_DATASOURCE_USERNAME
SPRING_DATASOURCE_PASSWORD
```

Make sure:

✔ RDS is publicly accessible
✔ EC2 security group allows port **3306**
✔ Username/password are correct

---

# 🌍 7. Can Docker Compose Run on Multiple Servers?

**No — Docker Compose works on a single server only.**

If you want multi-server deployment, use:

* **Docker Swarm** (easiest)
* **Kubernetes K3s / EKS**
* **Portainer Swarm mode**

---

# 🛑 8. Stopping the Application

```bash
docker-compose down
```

Or remove images:

```bash
docker-compose down --rmi all
