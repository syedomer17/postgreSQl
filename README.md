# PostgreSQL & Docker Setup Guide

## Prerequisites
Ensure your system is up to date:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installing Docker

1. **Uninstall old versions (if any):**
```bash
sudo apt remove docker docker-engine docker.io containerd runc
```

2. **Install dependencies:**
```bash
sudo apt install ca-certificates curl -y
```

3. **Add Docker’s official GPG key:**
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

4. **Add the Docker repository:**
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. **Install Docker & Plugins:**
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

6. **Start & Enable Docker:**
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

7. **Verify installation:**
```bash
docker --version
docker info
```

---

## Installing PostgreSQL (Native Ubuntu Installation)

1. **Install PostgreSQL:**
```bash
sudo apt install postgresql postgresql-contrib -y
```

2. **Start & Enable PostgreSQL:**
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

3. **Access PostgreSQL:**
```bash
sudo -u postgres psql
```

4. **Create a New User & Database:**
```sql
CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypassword';
CREATE DATABASE mydb OWNER myuser;
```

5. **Exit PostgreSQL:**
```sql
\q
```

---

## Running PostgreSQL in Docker

1. **Pull the PostgreSQL Image:**
```bash
docker pull postgres
```

2. **Run a PostgreSQL Container:**
```bash
docker run --name my_postgres -e POSTGRES_USER=myuser -e POSTGRES_PASSWORD=mypassword -e POSTGRES_DB=mydb -p 5432:5432 -d postgres
```

3. **Access the PostgreSQL Container:**
```bash
docker exec -it my_postgres psql -U myuser -d mydb
```

4. **Stopping & Removing the Container:**
```bash
docker stop my_postgres
docker rm my_postgres
```

---

## Using PostgreSQL

### Connecting via `psql` (Native Install)
```bash
psql -U myuser -d mydb -h localhost -p 5432
```

### Connecting via Docker
```bash
docker exec -it my_postgres psql -U myuser -d mydb
```

---

## Additional Commands

### Check Running Containers:
```bash
docker ps
```

### View PostgreSQL Logs:
```bash
docker logs my_postgres
```

### Restart PostgreSQL (Native):
```bash
sudo systemctl restart postgresql
```

---

## Uninstalling PostgreSQL

**For Native Installation:**
```bash
sudo apt remove --purge postgresql postgresql-contrib -y
sudo apt autoremove -y
sudo rm -rf /var/lib/postgresql
```

**For Docker Installation:**
```bash
docker stop my_postgres
docker rm my_postgres
docker rmi postgres
```

---

🚀 **Your PostgreSQL setup is ready!**
