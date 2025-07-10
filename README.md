# DES_Systems WordPress Docker Setup with Backup

## Overview

This project runs a WordPress site using Docker with the following services:
- **MariaDB** (database)
- **Redis** (cache)
- **WordPress** (PHP-FPM)
- **Nginx** (web server)
- **Backup** (cron-based backup container)

---

## Backup Location

Backups are saved on your Windows host at:

C:\Users\Micha\Desktop\alt0160\DES_Systems\des_systems_wp_backup


This folder contains:
- `db.sql` — Latest MariaDB database dump
- `wp_data/` — Backup of WordPress files and uploads

---

## Starting the Project

### First time (build images and start containers):

docker-compose up -d --build

    Builds all images, including the custom WordPress and backup containers.

    Starts containers in detached mode.

Subsequent starts (no rebuild):

docker-compose up -d

Stopping the Project

To safely stop all containers and keep data intact:

docker-compose down

    This stops all containers gracefully.

    Named Docker volumes (db_data and wp_data) preserve your database and files.

Managing Volumes & Data Persistence

    Database and WordPress files are stored in Docker volumes:

        db_data — MariaDB database files

        wp_data — WordPress website files

    Volumes are mounted read-only in the backup container for safety.

    Your backups folder syncs nightly with these volumes via the backup container.

Checking Volume Data Location

Docker Desktop stores volumes inside the WSL2 VM on Windows for performance:

    You typically do not need to manually interact with volume data.

    Use backups in the Windows folder for external access.

Avoiding Data Loss — Best Practices

    Do not delete Docker volumes manually unless you intend to lose data.

    Always stop containers using docker-compose down or via Docker Desktop UI.

    Keep the backup container running to perform nightly backups.

    Check backup logs with:

docker logs wp_backup

    Periodically verify backup folder contents on your Windows host.

    Before major updates or changes, consider manually copying backup files.

    Use Docker Desktop’s "Restart Docker Desktop" cautiously; it should preserve volumes.

Restoring from Backup

To restore your WordPress site from backups:

    Stop all containers:

docker-compose down

    Copy db.sql back into the MariaDB container using:

docker exec -i wp_mariadb mysql -u wp_user -pwp_pass wordpress < path/to/db.sql

    Copy WordPress files back to the volume (via docker cp or volume mounts).

    Start containers again:

docker-compose up -d

Additional Notes

    Redis cache is not backed up, as it stores ephemeral data.

    Backups run daily at midnight EST inside the backup container.

    Timezone is configurable via .env.
