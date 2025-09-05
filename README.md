# Docker Database Playground

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)

Welcome to the **Docker Database Playground**! This repository is your personal, pre-configured learning environment designed to help you master various database systems quickly and easily using Docker.

---

## ✨ Key Features

- **Quick & Easy:** Run popular databases like MySQL and PostgreSQL with a single command.
- **Secure by Default:** Secure credential management using `.env` files instead of hard-coding.
- **Comprehensive Guides:** Each database includes a learning guide covering everything from basic to intermediate topics like CRUD, Joins, Indexing, Transactions, and more.
- **GUI Ready:** Includes instructions for connecting your databases to modern GUI clients like Beekeeper Studio.
- **Batteries Included:** Comes with guides for backup/restore operations and user management.

---

## 🚀 Quick Start

Follow these steps to get your database of choice running in minutes.

1.  **Choose a Database**
    Navigate into the directory of the database you want to use.
    ```bash
    # Example for MySQL
    cd mysql/
    ```

2.  **Configure Your Environment**
    Copy the `.env.example` file to `.env`. You can edit this file to change passwords or ports if needed.
    ```bash
    cp .env.example .env
    ```

3.  **Start the Service**
    Use `docker-compose` to run the database in the background.
    ```bash
    docker-compose up -d
    ```
    Your database is now running and ready to use!

4.  **Stop the Service**
    When you're done, stop and remove the containers with:
    ```bash
    docker-compose down
    ```

---

## 📚 Databases & Learning Guides

This repository provides a complete environment and learning guide for the following databases:

| Database | Learning Guide | Backup Guide | Default Credentials |
| :--- | :--- | :--- | :--- |
| **MySQL** | [LEARNING_GUIDE.md](./mysql/LEARNING_GUIDE.md) | [BACKUP_RESTORE.md](./mysql/BACKUP_RESTORE.md) | `user` / `changeme` |
| **PostgreSQL** | [LEARNING_GUIDE.md](./Postgresql/LEARNING_GUIDE.md) | [BACKUP_RESTORE.md](./Postgresql/BACKUP_RESTORE.md) | `user` / `changeme` |

---

## 💻 Connecting with a GUI Client

You can use a GUI client like **Beekeeper Studio** to visually manage your database.

1.  **Download Beekeeper Studio:** [https://www.beekeeperstudio.io/](https://www.beekeeperstudio.io/)
2.  **Create a New Connection** using the following details (adjust with values from your `.env` file):
    - **Host:** `127.0.0.1` (or `localhost`)
    - **Port:** Default is `3306` for MySQL, `5432` for PostgreSQL.
    - **User/Password/Database:** Use the values from your `.env` file.

---

## 🛠️ Prerequisites

Ensure you have **Docker** and **Docker Compose** installed on your system.

- **Windows & macOS:** Install [Docker Desktop](https://www.docker.com/products/docker-desktop/). It includes Docker Compose.
- **Linux:** Follow the official guides to install the [Docker Engine](https://docs.docker.com/engine/install/#server) and the [Docker Compose Plugin](https://docs.docker.com/compose/install/linux/).

---

## 🔒 Security Note

The `.env` file contains sensitive information and is configured to be ignored by Git (via `.gitignore`). Never commit or publish your `.env` file.
