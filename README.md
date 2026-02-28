# OpenCanary Modular Honeynet

A lightweight, containerized honeynet deployment using [OpenCanary](https://github.com/thinkst/opencanary) and Docker Compose. This project simulates multiple internal network services designed to detect unauthorized access and lateral movement within a network.

## Features

This honeynet deploys four pre-configured service nodes:
*   **SSH (Port 22)**: Simulates a Linux SSH server.
*   **Web/HTTP (Port 80)**: Simulates an internal Apache admin server.
*   **SMB (Port 445)**: Simulates a Windows file sharing server.
*   **MySQL (Port 3306)**: Simulates a backend database server.

Each service runs in its own isolated Docker container based on the official `thinkst/opencanary` image, communicating under a shared host network or exposed ports.

## Architecture & Structure

The project relies on a single `docker-compose.yml` for orchestration, and mounts individual `.conf` files directly to the OpenCanary containers. Interaction logs from each honeypot are segregated into dedicated directories.

```text
honeynet/
├── docker-compose.yml      # Orchestration and container definitions
├── mysql/                  # Configuration for the Database node
├── smb/                    # Configuration for the File Sharing node
├── ssh/                    # Configuration for the SSH server node
├── web/                    # Configuration for the Web server node
└── logs/                   # Segregated Interaction Logs
    ├── mysql/
    ├── smb/
    ├── ssh/
    └── web/
```

## Getting Started

### Prerequisites
*   [Docker](https://docs.docker.com/get-docker/) installed.
*   Docker Compose installed.

### Running the Honeynet

To start all honeypot services in detached mode, simply run:

```bash
cd honeynet/
docker compose up -d
```

To gracefully stop the honeynet:

```bash
docker compose down
```

## Testing and Verification

Once the containers are running, you can actively test connections to ensure the honeypots are successfully logging the events. 

For comprehensive instructions on how to test each service and view the generated logs, please refer to the [Testing Guide (testing.md)](testing.md).

## License
This project is for educational and defensive security purposes. OpenCanary is an open-source project by Thinkst Applied Research.
