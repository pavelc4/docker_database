# Docker Database Playground

[ English | **Bahasa Indonesia** ]

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)

Selamat datang di **Docker Database Playground**! Repositori ini adalah lingkungan belajar pribadi Anda yang dirancang untuk membantu Anda menguasai berbagai sistem database dengan cepat dan mudah menggunakan Docker.

---

## ✨ Fitur Utama

- **Cepat & Mudah:** Jalankan database populer seperti MySQL dan PostgreSQL dengan satu perintah.
- **Konfigurasi Aman:** Pengaturan kredensial yang aman menggunakan file `.env`, bukan *hard-coding*.
- **Panduan Komprehensif:** Setiap database dilengkapi dengan panduan belajar dari dasar hingga menengah, mencakup CRUD, *Joins*, *Indexing*, Transaksi, dan lainnya.
- **Siap GUI:** Termasuk panduan untuk menghubungkan database Anda ke klien GUI modern seperti Beekeeper Studio.
- **Lengkap:** Dilengkapi dengan panduan backup/restore dan manajemen pengguna.

---

## 🚀 Quick Start

Ikuti langkah-langkah ini untuk menjalankan database pilihan Anda dalam hitungan menit.

1.  **Pilih Database**
    Masuk ke direktori database yang Anda inginkan.
    ```bash
    # Contoh untuk MySQL
    cd mysql/
    ```

2.  **Konfigurasi Lingkungan**
    Salin file `.env.example` menjadi `.env`. Anda bisa mengubah *password* atau *port* di dalam file ini jika perlu.
    ```bash
    cp .env.example .env
    ```

3.  **Jalankan Service**
    Gunakan `docker-compose` untuk menjalankan database di latar belakang.
    ```bash
    docker-compose up -d
    ```
    Database Anda sekarang berjalan dan siap digunakan!

4.  **Hentikan Service**
    Jika sudah selesai, matikan dan hapus *container* dengan perintah:
    ```bash
    docker-compose down
    ```

---

## 📚 Database & Panduan Belajar

Repositori ini menyediakan lingkungan dan panduan belajar yang lengkap untuk database berikut:

| Database | Panduan Belajar | Panduan Backup | Kredensial Default |
| :--- | :--- | :--- | :--- |
| **MySQL** | [LEARNING_GUIDE.id.md](./mysql/LEARNING_GUIDE.id.md) | [BACKUP_RESTORE.id.md](./mysql/BACKUP_RESTORE.id.md) | `user` / `changeme` |
| **PostgreSQL** | [LEARNING_GUIDE.id.md](./Postgresql/LEARNING_GUIDE.id.md) | [BACKUP_RESTORE.id.md](./Postgresql/BACKUP_RESTORE.id.md) | `user` / `changeme` |

---

## 💻 Menghubungkan dengan Klien GUI

Anda dapat menggunakan klien GUI seperti **Beekeeper Studio** untuk mengelola database secara visual.

1.  **Unduh Beekeeper Studio:** [https://www.beekeeperstudio.io/](https://www.beekeeperstudio.io/)
2.  **Buat Koneksi Baru** menggunakan detail berikut (sesuaikan dengan nilai di file `.env` Anda):
    - **Host:** `127.0.0.1` (atau `localhost`)
    - **Port:** Default `3306` untuk MySQL, `5432` untuk PostgreSQL.
    - **User/Password/Database:** Gunakan nilai yang ada di file `.env` Anda.

---

## 🛠️ Prasyarat Instalasi

Pastikan **Docker** dan **Docker Compose** sudah terpasang di sistem Anda.

- **Windows & macOS:** Install [Docker Desktop](https://www.docker.com/products/docker-desktop/). Docker Compose sudah termasuk di dalamnya.
- **Linux:** Ikuti panduan resmi untuk menginstall [Docker Engine](https://docs.docker.com/engine/install/#server) dan [Docker Compose Plugin](https://docs.docker.com/compose/install/linux/).

---

## 🔒 Catatan Keamanan

File `.env` berisi informasi sensitif dan sudah diatur untuk diabaikan oleh Git (via `.gitignore`). Jangan pernah membagikan atau mem-publish file `.env` Anda.
