# Homelab Infrastructure

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-ff6727?style=for-the-badge&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

Infrastructure as Code (IaC) repository for managing my personal homelab via **Kubernetes (K3s)** and **Ansible**.
This project implements GitOps practices to maintain the state of the home infrastructure, ensuring reliability, security, and reproducibility.

## Tech Stack

| Category | Technology | Description |
|----------|------------|-------------|
| **OS** | Ubuntu Server 24.04 LTS | Base operating system |
| **Orchestration** | K3s | Lightweight Kubernetes distribution |
| **Config Management** | Ansible | Node provisioning and configuration |
| **CI/CD** | GitHub Actions | Automated linting and testing pipelines |
| **GitOps** | ArgoCD | Continuous Delivery for K8s manifests |
| **Security** | Fail2Ban, Ansible Vault | Intrusion prevention and secret management |
| **Ingress** | Traefik | Edge router and Load Balancer |

## Architecture

The infrastructure acts as a central hub for network filtering, home automation, and media services.

```
[ ISP / Internet ]
      │
      ▼
[ Router / Gateway ]
      │
      ├── WIRED (Ethernet)
      │     │
      │     ├── [ Multimedia Clients ] (Smart TV, etc.)
      │     │
      │     └── [ HOMELAB SERVER ] (ThinkPad L480)
      │            │
      │            ├── OS: Ubuntu Server
      │            │
      │            └── K3s Cluster (Single Node)
      │                   ├── Namespace: adguard-home
      │                   │     └─ Pod: AdGuard Home (Network-wide DNS)
      │                   │
      │                   ├── Namespace: home-automation
      │                   │     └─ Pod: Home Assistant (Planned)
      │                   │
      │                   ├── Namespace: media
      │                   │     └─ Pod: Jellyfin (Planned)
      │                   │
      │                   └── System: Traefik (Ingress), ArgoCD (GitOps)
      │
      └── WIRELESS (Wi-Fi)
            │
            ├── [ Workstation ] (Control Node)
            │      └─ Tools: Ansible, kubectl, git
            │
            └── [ IoT Infrastructure ]
                   ├─ Device 01 (Environmental Sensors)
                   ├─ Device 02 (Camera)
                   └─ ... (Scalable Fleet)
```

# Hardware Specs
The cluster runs on bare metal hardware, repurposed for energy efficiency.

- **Node:** Lenovo ThinkPad L480
- **CPU:** Intel Core i5-8250U (4 cores / 8 threads)
- **RAM:** 8GB DDR4
- **Storage:** NVMe SSD
- **Power:** Integrated UPS (Laptop Battery)
