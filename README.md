# Docker Database

This repository provides Docker Compose configurations to easily set up and run MySQL and PostgreSQL database services.

---

## 1. Installation Prerequisites

Before using this project, you need Docker and Docker Compose installed on your system. Docker Desktop for Windows and macOS includes Docker Compose automatically.

### macOS
- Install **Docker Desktop for Mac** by following the official guide:
  - [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)

### Windows
- Install **Docker Desktop for Windows** by following the official guide. You will need WSL 2 (Windows Subsystem for Linux).
  - [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)

### Linux
- For most Linux distributions, you will need to install the Docker Engine and then the Docker Compose plugin separately.

- **1. Install Docker Engine:**
  - **Ubuntu:** [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)
  - **Debian:** [https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)
  - **CentOS:** [https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)
  - **Fedora:** [https://docs.docker.com/engine/install/fedora/](https://docs.docker.com/engine/install/fedora/)
  - **Arch Linux:** [https://wiki.archlinux.org/title/Docker](https://wiki.archlinux.org/title/Docker)

- **2. Install Docker Compose Plugin:**
  - Follow the official guide for installing the Compose plugin:
    - [https://docs.docker.com/compose/install/linux/](https://docs.docker.com/compose/install/linux/)

---

## 2. How to Use

1.  **Navigate to a database directory:**
    ```bash
    # For example, for MySQL:
    cd repo/mysql
    ```

2.  **Configure Environment Variables:**
    Each database directory contains a `.env.example` file. Before starting the service, you must create your own `.env` file from the example.
    ```bash
    # Copy the example file
    cp .env.example .env
    ```
    You can now edit the `.env` file to change passwords, ports, or database names.

3.  **Start the Database Service:**
    Run the following command to start the container in the background.
    ```bash
    docker-compose up -d
    ```

4.  **Stop the Database Service:**
    To stop and remove the containers, run the following command from the same directory:
    ```bash
    docker-compose down
    ```

---

## 3. Connecting with a GUI Client (Beekeeper Studio)

While you can manage the database from the command line, using a GUI client is often more convenient. We recommend **Beekeeper Studio**, a free and open-source client with a modern UI.

1.  **Download and Install Beekeeper Studio:**
    - Go to the official website and download the version for your OS: [https://www.beekeeperstudio.io/](https://www.beekeeperstudio.io/)

2.  **Connect to a Database:**
    - Make sure your Docker container is running (`docker-compose up -d`).
    - Open Beekeeper Studio and click `Import Connection` or the `+` icon to create a new connection.
    - Use the details below, taking the values from the `.env` file in your chosen database directory (`mysql/.env` or `Postgresql/.env`).


### MySQL Connection Details
- **Connection Type:** `MySQL`
- **Host:** `127.0.0.1` (or `localhost`)
- **Port:** The `MYSQL_PORT` value from your `.env` file (default: `3306`).
- **User:** The `MYSQL_USER` value from your `.env` file (default: `user`).
- **Password:** The `MYSQL_PASSWORD` value from your `.env` file (default: `changeme`).
- **Database:** The `MYSQL_DATABASE` value from your `.env` file (default: `testing_db`).

### PostgreSQL Connection Details
- **Connection Type:** `PostgreSQL`
- **Host:** `127.0.0.1` (or `localhost`)
- **Port:** The `POSTGRES_PORT` value from your `.env` file (default: `5432`).
- **User:** The `POSTGRES_USER` value from your `.env` file (default: `user`).
- **Password:** The `POSTGRES_PASSWORD` value from your `.env` file (default: `changeme`).
- **Database:** The `POSTGRES_DB` value from your `.env` file (default: `example_db`).

---

## 4. Available Databases & Learning Guides

- **MySQL:** See the [MySQL README](./mysql/README.md) for setup and connection details.
- **PostgreSQL:** See the [PostgreSQL README](./Postgresql/README.md) for setup and connection details.

Each database folder contains its own `docker-compose.yml`, learning guides, and backup instructions.

---

## 5. Security Note

The `.env` file should not be committed to version control. This repository's `.gitignore` file is already configured to ignore `.env` files.
