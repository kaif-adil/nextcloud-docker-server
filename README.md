# Nextcloud Remote Server Setup (Docker)

> This project was built to understand containerized deployment and persistent storage in real-world self-hosted systems.

## Overview
This repository documents my hands-on experience setting up a self-hosted cloud file server using Nextcloud (Apache) inside a Docker container on Ubuntu Linux.
The focus was on ensuring data persistence by mounting an external storage device so that data remains intact even if the container is recreated.

---

## Tech Stack
- Docker (containerization)
- Nextcloud (Apache image)
- Ubuntu Linux
- External storage (EXT4 filesystem)

---

## What This Project Demonstrates
- Containerized service deployment using Docker
- Persistent storage using external drives
- Linux filesystem and permission management
- Local access to self-hosted services via browser

---

## Setup Steps

### 1. Run Nextcloud (Basic Test)
```bash
docker run -d -p 8080:80 nextcloud
```

---

### 2. Plug in External Drive and Identify It
```bash
lsblk
```

Assume the drive appears as:
```
/dev/sda1
```

---

### 3. Format the Drive (EXT4)
```bash
sudo mkfs.ext4 /dev/sda1
```

---

### 4. Create a Mount Point
```bash
sudo mkdir -p /mnt/usb_drive
```

---

### 5. Mount the Drive
```bash
sudo mount -o rw /dev/sda1 /mnt/usb_drive
```

---

### 6. Set Permissions for Nextcloud
Nextcloud runs as the `www-data` user (UID 33).

```bash
sudo chown -R 33:33 /mnt/usb_drive
sudo chmod -R 0777 /mnt/usb_drive
```

---

### 7. Run Nextcloud with Persistent Storage
```bash
docker run -d \
  --name new_nextcloud \
  -p 8080:80 \
  -v /mnt/usb_drive:/var/www/html/data \
  nextcloud:apache
```

---

### 8. Access the Server
Open your browser and navigate to:
```
http://localhost:8080/
```

Complete the Nextcloud setup through the web interface.

---

## Key Learnings
- Docker volumes are essential for data persistence
- Linux file permissions directly affect container behavior
- Containers can be safely recreated without losing data
- Self-hosted cloud services are achievable with minimal tooling

---

## Future Improvements
- Docker Compose for easier service management
- Automatic drive mounting using `/etc/fstab`
- HTTPS using a reverse proxy
- Backup and access control configuration
