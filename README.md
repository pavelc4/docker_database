# Docker Database

This repository provides Docker Compose configurations to easily set up and run MySQL and PostgreSQL database services.

## Prerequisites

Before you begin, ensure you have the following installed on your system:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Available Databases

- **MySQL:** See the [MySQL README](./mysql/README.md) for setup and connection details.
- **PostgreSQL:** See the [PostgreSQL README](./Postgresql/README.md) for setup and connection details.

Each database folder contains its own `docker-compose.yml`, learning guides, and backup instructions.

---

## How to Use

1.  **Navigate to a database directory:**
    ```bash
    # For example, for MySQL:
    cd repo/mysql
    ```

2.  **Configure Environment Variables:**
    Each database directory contains a `.env.example` file. Before starting the service, you must create your own `.env` file.
    ```bash
    # Copy the example file
    cp .env.example .env
    ```
    Now, you can edit the `.env` file to change passwords, ports, or database names as needed.

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

## Security Note

The `.env` file should not be committed to version control. This repository's `.gitignore` file is already configured to ignore `.env` files.
