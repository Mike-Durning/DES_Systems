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


| **Command Purpose**                                | **Command**                                | **Explanation**                                                                       |
| -------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| 1. Shut down containers **keeping volumes**        | `docker-compose down`                      | Stops and removes containers and networks but **keeps volumes intact** (no `-v` flag) |
| 3. Manually execute backup script inside container | `docker exec wp_backup /scripts/backup.sh` | Runs the backup script inside the running backup container to create backups          |


