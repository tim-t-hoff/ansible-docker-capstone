# Automated High-Availability Lab Orchestration

A Senior Capstone project demonstrating automated infrastructure deployment using Ansible and Docker-in-Docker (DinD).

## Project Overview
This project automates the deployment of a high-availability web cluster. It uses Ansible to orchestrate a custom Nginx Load Balancer and multiple web application nodes within a simulated environment.

## Architecture
- **Ansible Control Plane:** Manages configuration via Playbooks and Jinja2 templates.
- **Docker-in-Docker (DinD):** Simulates remote managed nodes as containers to avoid cloud costs.
- **Load Balancer:** Custom Nginx build with dynamic upstream configuration.

## Setup
1. Make an ssh key pair with a remote server from the device you plan to run Ansible on, and put the public key in the `/Dockerfile`
2. Configure the variables in `/inventory.ini` such as ansible_host IP's and ports for the apps and Ansible. 

## How to Run
1. Start the lab environment: `docker compose up -d`
2. Run the Ansible Playbook: `ansible-playbook main.yml`

## How to Reset
1. Run `docker compose down` to stop and remove the containers, resetting them
2. Run `docker compose up` to start the containers again, in a clean state

## Logic Flow
1. **Bootstrap:** Install Docker and dependencies on all nodes.
2. **Deploy Apps:** Spin up Nginx web containers.
3. **Deploy LB:** Build and start the custom Load Balancer.
4. **Cleanup:** Prune unused Docker resources.

## Architecture Diagram
![Architecture Diagram](./media/Automated%20Container%20Deployment%20Architecture%20Diagram.jpg)