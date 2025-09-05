# Docker Database

This repository provides Docker Compose configurations to easily set up and run MySQL and PostgreSQL database services.

## Prerequisites

Before you begin, ensure you have the following installed on your system:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Available Databases

- [MySQL](./mysql/)
- [PostgreSQL](./Postgresql/)

---

## How to Use

The instructions are the same for both Linux, macOS, and Windows.

### 1. Choose a Database

Navigate to the directory of the database you want to use.

- **For MySQL:**
  ```bash
  cd mysql
  ```

- **For PostgreSQL:**
  ```bash
  cd Postgresql
  ```

### 2. Start the Database Service

Run the following command to build the image and start the container in the background (detached mode).

```bash
docker-compose up -d
```

### 3. Stop the Database Service

To stop and remove the running containers, execute the following command from the same directory:

```bash
docker-compose down
```

---

## Database Connection Details

Use a database client of your choice to connect to the running service.

### MySQL

- **Host:** `localhost`
- **Port:** `3306`
- **User:** `aether`
- **Password:** `1945`
- **Database:** `testing_db`
- **Root Password:** `1945`

### PostgreSQL

- **Host:** `localhost`
- **Port:** `5432`
- **User:** `aether`
- **Password:** `1945`
- **Database:** `example_db`