# Docker and PostgreSQL Setup Guide

## Install Docker

### 1. Update Packages and Install Dependencies
```sh
sudo apt update
sudo apt install ca-certificates curl -y
```

### 2. Add Docker’s GPG Key
```sh
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### 3. Add Docker Repository
```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu noble stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Install Docker Engine and Plugins
```sh
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

### 5. Verify Installation
```sh
docker --version
docker compose version
```

## Install PostgreSQL using Docker

### 1. Pull PostgreSQL Image
```sh
docker pull postgres:latest
```

### 2. Run PostgreSQL Container
```sh
docker run --name postgres_db -e POSTGRES_USER=myuser -e POSTGRES_PASSWORD=mypassword -e POSTGRES_DB=mydatabase -p 5432:5432 -d postgres
```

### 3. Check Running Containers
```sh
docker ps
```

### 4. Connect to PostgreSQL Container
```sh
docker exec -it postgres_db psql -U myuser -d mydatabase
```

## Common Docker Commands

### Start a Container
```sh
docker start <container_id_or_name>
```

### Stop a Container
```sh
docker stop <container_id_or_name>
```

### Restart a Container
```sh
docker restart <container_id_or_name>
```

### Remove a Container
```sh
docker rm <container_id_or_name>
```

### List All Containers
```sh
docker ps -a
```

### View Logs of a Container
```sh
docker logs <container_id_or_name>
```

### Enter the PostgreSQL Container
```sh
docker exec -it postgres_db bash
```

### Stop and Remove All Containers
```sh
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
```

### Remove All Docker Images
```sh
docker rmi $(docker images -q)
```

## Using PostgreSQL Inside the Container

### 1. Connect to PostgreSQL CLI
```sh
docker exec -it postgres_db psql -U myuser -d mydatabase
```

### 2. Create a Table
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
```

### 3. Insert Data
```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### 4. Query Data
```sql
SELECT * FROM users;
```

### 5. Exit PostgreSQL
```sh
\q
```

## Uninstall Docker
```sh
sudo apt remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo rm -rf /var/lib/docker
