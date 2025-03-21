# postgreSQl

# PostgreSQL with Docker - Setup Guide

This guide will help you set up PostgreSQL inside a Docker container with persistent storage.

## Prerequisites
- Ensure **Docker** is installed. If not, install it with:
  ```bash
  sudo apt update
  sudo apt install docker.io -y
  ```
- Start and enable Docker:
  ```bash
  sudo systemctl start docker
  sudo systemctl enable docker
  ```
- Verify Docker installation:
  ```bash
  docker --version
  ```

## Step 1: Pull the PostgreSQL Docker Image
```bash
docker pull postgres
```

## Step 2: Run PostgreSQL Container
```bash
docker run --name postgres-container \
  -e POSTGRES_USER=omer \
  -e POSTGRES_PASSWORD="omer@2006" \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  -d postgres
```

## Step 3: Verify the Container is Running
```bash
docker ps
```
If the container is stopped, check all containers:
```bash
docker ps -a
```

## Step 4: Connect to PostgreSQL
```bash
docker exec -it postgres-container psql -U omer -d mydatabase
```
Test connection inside PostgreSQL:
```sql
SELECT NOW();
```

## Step 5: Manage the PostgreSQL Container
- **Stop the Container:**
  ```bash
  docker stop postgres-container
  ```
- **Start the Container:**
  ```bash
  docker start postgres-container
  ```
- **View Container Logs:**
  ```bash
  docker logs postgres-container
  ```
- **Remove the Container:**
  ```bash
  docker rm -f postgres-container
  ```

## Step 6: Persistent Storage (Recommended)
To persist data, create a volume:
```bash
docker volume create pgdata
```
Run PostgreSQL with persistent storage:
```bash
docker run --name postgres-container \
  -e POSTGRES_USER=omer \
  -e POSTGRES_PASSWORD="omer@2006" \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  -d postgres
```

## Step 7: Connect PostgreSQL to GUI (pgAdmin/DBeaver)
- **Host:** `localhost`
- **Port:** `5432`
- **Username:** `omer`
- **Password:** `omer@2006`
- **Database:** `mydatabase`

## Done 🎉
You now have PostgreSQL running inside Docker. 🚀
