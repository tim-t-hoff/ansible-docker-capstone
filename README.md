# Automated High-Availability Lab Orchestration

A Senior Capstone project demonstrating automated infrastructure deployment using Ansible and Docker-in-Docker (DinD).

## Project Overview
This project automates the deployment of a high-availability web cluster. It uses Ansible to orchestrate a custom Nginx Load Balancer and multiple web application nodes within a simulated environment.

##

**The Goal:** To demonstrate how Infrastructure-as-Code (IaC) can manage complex network topologies and service redundancy without the overhead of multiple physical or cloud-based virtual machines.

## Key Features
* **Infrastructure-as-Code:** Full lifecycle management from system bootstrapping to service deployment.
* **Dynamic Load Balancing:** Automated Nginx configuration using Jinja2 templates that scale based on the Ansible inventory.
* **Simulated Multi-Node Environment:** Utilizes a "Docker-in-Docker" (DinD) architecture to mimic remote managed nodes.
* **High Availability:** Implemented round-robin traffic distribution with easy "stop/start" management playbooks.

## Architecture
- **Ansible Control Plane:** Manages configuration via Playbooks and Jinja2 templates.
- **Docker-in-Docker (DinD):** Simulates remote managed nodes as containers to avoid cloud costs.
- **Load Balancer:** Custom Nginx build with dynamic upstream configuration.

## Architecture Diagram
![Architecture Diagram](./media/Automated%20Container%20Deployment%20Architecture%20Diagram.jpg)

## Setup & Prerequisites
1.  **SSH Authentication:** Generate an SSH key pair on your management machine. Place the public key in `remote-server/Dockerfile` under the authorized_keys section.
2.  **Inventory Configuration:** Update `inventory.ini` with your target host IPs and preferred port mappings for the web applications.
3.  **Dependencies:** Ensure Docker and Ansible are installed on your local control machine.

## Execution Flow
1.  **Initialize Lab:** `docker compose up -d` (This creates the simulated "servers").
2.  **Deploy Stack:** `ansible-playbook main.yml` (This configures the servers and starts the apps).
3.  **Manage Apps:** Use `ansible-playbook manage-apps.yml -e "target_state=stop"` to test failover.

## Resetting the Environment
To return to a clean "Day 0" state:
```
docker compose down
docker compose up -d
```

## Logic Flow
1. **Bootstrap:** Install Docker engine and Python dependencies on all simulated nodes.
2. **Web Deployment:** Provision Nginx web containers with unique identifiers.
3. **LB Integration:** Build the custom Load Balancer image and deploy it to the bridge network.
4. **Cleanup: Execute** a prune task to remove unused build layers and maintain host storage.
