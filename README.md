# PostgreSQL Setup & Commands in WSL (Windows Subsystem for Linux)

This guide covers how to install, set up, and use PostgreSQL in **WSL (Windows Subsystem for Linux)**, including database creation, management, and connecting it to Node.js in VS Code.

---

## 📌 **1. Install PostgreSQL in WSL**

### 🔹 **Update Package Lists**
```sh
sudo apt update && sudo apt upgrade -y
```
**Explanation:** Ensures your system packages are up to date.

### 🔹 **Install PostgreSQL**
```sh
sudo apt install postgresql postgresql-contrib -y
```
**Explanation:** Installs PostgreSQL and additional utilities.

### 🔹 **Start PostgreSQL Service**
```sh
sudo service postgresql start
```
**Explanation:** Starts the PostgreSQL service.

### 🔹 **Enable PostgreSQL to Start on Boot**
```sh
sudo systemctl enable postgresql
```
**Explanation:** Ensures PostgreSQL starts automatically.

---

## 📌 **2. Access PostgreSQL in WSL**

### 🔹 **Switch to the PostgreSQL User**
```sh
sudo -i -u postgres
```
**Explanation:** Logs in as the `postgres` superuser.

### 🔹 **Open PostgreSQL Shell (psql)**
```sh
psql
```
**Explanation:** Opens the PostgreSQL interactive terminal.

### 🔹 **Exit PostgreSQL Shell**
```sql
\q
```
**Explanation:** Exits the `psql` terminal.

---

## 📌 **3. Database Management**

### 🔹 **Create a New Database**
```sql
CREATE DATABASE my_database;
```
**Explanation:** Creates a database named `my_database`.

### 🔹 **List All Databases**
```sql
\l
```
**Explanation:** Displays a list of databases.

### 🔹 **Connect to a Database**
```sh
psql -d my_database
```
**Explanation:** Connects to `my_database`.

### 🔹 **Drop (Delete) a Database**
```sql
DROP DATABASE my_database;
```
**Explanation:** Permanently deletes `my_database`.

---

## 📌 **4. User & Role Management**

### 🔹 **Create a New PostgreSQL User**
```sql
CREATE USER my_user WITH PASSWORD 'mypassword';
```
**Explanation:** Creates a new user with login access.

### 🔹 **Grant Privileges to a User**
```sql
GRANT ALL PRIVILEGES ON DATABASE my_database TO my_user;
```
**Explanation:** Grants all privileges on `my_database` to `my_user`.

### 🔹 **Delete a PostgreSQL User**
```sql
DROP USER my_user;
```
**Explanation:** Removes `my_user` from PostgreSQL.

---

## 📌 **5. Table Management**

### 🔹 **Create a Table**
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255) UNIQUE NOT NULL,
    age INT
);
```
**Explanation:** Creates a `users` table.

### 🔹 **List Tables in a Database**
```sql
\dt
```
**Explanation:** Shows all tables in the current database.

### 🔹 **Drop a Table**
```sql
DROP TABLE users;
```
**Explanation:** Deletes the `users` table.

---

## 📌 **6. Start, Stop, and Restart PostgreSQL in WSL**

### 🔹 **Start PostgreSQL**
```sh
sudo service postgresql start
```
**Explanation:** Starts PostgreSQL.

### 🔹 **Stop PostgreSQL**
```sh
sudo service postgresql stop
```
**Explanation:** Stops PostgreSQL.

### 🔹 **Restart PostgreSQL**
```sh
sudo service postgresql restart
```
**Explanation:** Restarts PostgreSQL.

---

## 📌 **7. Connect PostgreSQL to Node.js in VS Code**

### 🔹 **Install Required Dependencies**
```sh
npm install pg dotenv
```
**Explanation:** Installs `pg` (PostgreSQL client for Node.js) and `dotenv` for environment variables.

### 🔹 **Create a `.env` File**
```env
DATABASE_URL=postgres://my_user:mypassword@localhost:5432/my_database
```
**Explanation:** Stores the database connection string.

### 🔹 **Connect to PostgreSQL in Node.js**
Create a file `db.js` in your project and add:
```js
import { Pool } from 'pg';
import dotenv from 'dotenv';

dotenv.config();

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

export default pool;
```
**Explanation:** Sets up a connection pool using environment variables.

### 🔹 **Test the Connection**
Create a file `test.js`:
```js
import pool from './db.js';

(async () => {
  try {
    const res = await pool.query('SELECT NOW();');
    console.log('Database connected at:', res.rows[0].now);
  } catch (err) {
    console.error('Database connection error:', err);
  }
})();
```
**Explanation:** Runs a simple query to check the connection.

Run the test file with:
```sh
node test.js
```

---

## 🎯 **Conclusion**
This guide helps you install, configure, and manage PostgreSQL inside WSL, as well as connect it to Node.js in VS Code. Let me know if you need additional steps! 🚀
