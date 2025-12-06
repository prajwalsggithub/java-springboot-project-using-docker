
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



# ▶️ 2. Running the Application

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

# 🌐 3. Accessing the Services

| Service  | URL                          |
| -------- | ---------------------------- |
| Frontend | `http://YOUR_SERVER_IP:8501` |
| Backend  | `http://YOUR_SERVER_IP:8084` |

Replace `YOUR_SERVER_IP` with your EC2 / VPS IP.

---

# 🗄️ 4. Connecting to AWS RDS

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

# 🌍 5. Can Docker Compose Run on Multiple Servers?

**No — Docker Compose works on a single server only.**

If you want multi-server deployment, use:

* **Docker Swarm** (easiest)
* **Kubernetes K3s / EKS**
* **Portainer Swarm mode**

---

# 🛑 6. Stopping the Application

```bash
docker-compose down
```

Or remove images:

```bash
docker-compose down --rmi all
