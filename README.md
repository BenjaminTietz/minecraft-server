# Minecraft Server with Docker & mcstatus

## 📘 Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Project Structure](#project-structure)
4. [Quickstart](#quickstart)
5. [Usage and Configuration](#usage-and-configuration)
6. [Verify Server Status](#verify-server-status-without-minecraft-client)
7. [Minimal docker-compose.yml](#minimal-docker-composeyml)
8. [Stop and Cleanup](#stop-and-cleanup)
9. [Contact](#contact)
10. [Checklist](checklist.pdf)

---

## Introduction

This repository provides a minimal Docker-based setup for running a Minecraft server using Docker Compose. It demonstrates containerization, volume persistence, and external availability checks via `mcstatus` — without requiring the Minecraft game client or in-depth knowledge of the game itself.

---

# Setup

## Prerequisites

- A V-Server running Ubuntu/Debian
- Docker

Ensure your system is up to date:

```sh
sudo apt update && sudo apt install -y docker.io docker-compose git
```

---

### Project Structure

```
minecraft-server/
├── docker-compose.yml
├── .gitignore
└── data/                # Automatically created when the server runs it includes all the server data including settings etcpp
```

---

### Quickstart

1. **Install dependencies:**
   ```sh
   sudo apt update && sudo apt install -y docker.io docker-compose git
   ```
2. **Clone the repository:**
   ```sh
   git clone git@github.com:BenjaminTietz/minecraft-server.git
   cd mincraft-server
   ```
3. **Generate and configure the .env file:** <br>
   The environment file will be created automatically from env.template.
   Adjust the values to match your setup (optional):
   ```sh
   cp env_template.txt .env
   nano .env (optional)
   ```
4. Start the Minecraft server:
   ```bash
   docker-compose up -d
   ```
5. Install `mcstatus` and check server status:
   ```bash
   python -m pip install mcstatus
   mcstatus localhost:25565 status
   ```

---

## Usage and Configuration

You can configure the server using environment variables in `.env`.

### Default environment:

```yaml
EULA: "TRUE" # Required to accept Minecraft EULA
MOTD: "Welcome to my server!" # Message of the Day
```

### Volumes

All world data and configuration are persisted in the `./data` folder:

```yaml
volumes:
  - ./data:/data
```

### Ports

The default Minecraft port is exposed:

```yaml
ports:
  - "25565:25565"
```

For cloud deployments, you may map it to another port, e.g., `8888:25565`.

---

## Verify Server Status (without Minecraft client)

### Install mcstatus:

```bash
python -m pip install mcstatus
```

### Check server status:

```bash
mcstatus localhost:25565 status
```

### Example output:

```bash
version: v1.21.5 (protocol 770)
motd: "A Minecraft Server"
players: 0/20 No players online
```

---

## Minimal docker-compose.yml

```yaml
services:
  minecraft-server:
    image: itzg/minecraft-server #This is an offical commuity image
    container_name: mc-server
    ports:
      - "25565:25565"
    environment:
      EULA: "TRUE" # (optional) use an .env
    volumes:
      - ./data:/data
    restart: on-failure
```

---

## Stop and Cleanup

```bash
docker-compose down
```

To remove all persisted data:

```bash
rm -rf ./data
```

---

## Contact

### 👤 Personal

- [Portfolio](https://benjamin-tietz.com/)
- [Drop me a mail](mailto:mail@benjamin-tietz.com)

### 🌍 Social

- [LinkedIn](https://www.linkedin.com/in/benjamin-tietz/)

### 💻 Project Repository

- [GitHub Repository](https://github.com/BenjaminTietz/minecraft-server)
