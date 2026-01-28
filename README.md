# Homelab Infrastructure

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/helm-%230F1689.svg?style=for-the-badge&logo=helm&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-ff6727?style=for-the-badge&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

Infrastructure as Code (IaC) monorepo for managing my personal homelab via **Kubernetes (K3s)** and **Ansible**.
This project implements **GitOps (ArgoCD)** practices to maintain the state of the home infrastructure, ensuring reliability, security and reproducibility.

## Tech Stack

| Category | Technology | Description |
|----------|------------|-------------|
| **OS** | Ubuntu Server 24.04 LTS | Base operating system |
| **Orchestration** | K3s | Lightweight Kubernetes distribution |
| **Package Manager** | Helm | Kubernetes package management |
| **Config Management** | Ansible | Node provisioning and configuration |
| **CI** | GitHub Actions | Automated linting and validation pipelines |
| **GitOps (CD)** | ArgoCD | Continuous Delivery for K8s manifests |
| **Security** | Fail2Ban (Custom IPS), Ansible Vault, Gitleaks, Trivy | Intrusion prevention and secret management |
| **Ingress** | Traefik | Edge router and Load Balancer |

## Architecture

The homelab server acts as a central hub for network filtering, home automation and media services.

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
      │                   ├── Namespace: home-assistant
      │                   │     └─ Pod: Home Assistant (Home automation)
      │                   │
      │                   ├── Namespace: jellyfin
      │                   │     └─ Pod: Jellyfin (Planned)
      │                   │
      │                   └── System: Traefik (Ingress), ArgoCD (GitOps)
      │
      └── WIRELESS (Wi-Fi)
            │
            ├── [ Workstation ] (Control Node)
            │      └─ Tools: Ansible, kubectl, git
            │
            └── [ IoT Devices ]
                   ├─ Device 01 (Environmental Sensor)
                   ├─ Device 02 (Camera)
                   └─ ... (Scalable Fleet)
```

## Directory Structure

```
.
├── .github/             # GitHub Actions workflows (CI)
├── ansible/             # Ansible playbooks and roles
│   ├── inventory/       # Hosts and group variables (encrypted)
│   ├── playbooks/       # Playbooks: bootstrap, etc. 
│   └── roles/           # Roles: docker, k3s, security, etc.
├── docs/                # Documentation and diagrams
└── k8s/                 # Kubernetes manifests (GitOps source)
    └── apps/            # Application definitions (Helm)
```

## Hardware Specs
The cluster runs on bare metal hardware, repurposed for energy efficiency and UPS capabilities.

- Node: Lenovo ThinkPad L480
- CPU: Intel Core i5-8250U (4 cores / 8 threads)
- RAM: 8GB DDR4
- Storage: NVMe SSD
- Power: Integrated UPS (Laptop Battery)

## Getting Started

To bootstrap the infrastructure from scratch:

1. **Clone the repository:**
```bash
git clone https://github.com/tiltaslifestyle/homelab.git
cd homelab
```

2. **Configure Ansible Secrets:** Create `ansible/inventory/group_vars/lab_servers.yml` based on the example file:
```bash
cp ansible/inventory/group_vars/lab_servers.yml.example ansible/inventory/group_vars/lab_servers.yml
```
*Edit the file with your Vault password*

3. **Run Ansible Bootstrap:**
```bash
ansible-playbook ansible/playbooks/bootstrap.yml --ask-vault-pass
```
*At this stage, the Kubernetes cluster is running, and ArgoCD is installed but idle.*

4.  **Kickstart GitOps:**

    * **Option A: Public Repository** <br>
        Edit `k8s/bootstrap.yaml` to point to your repository URL, then apply it:
        ```bash
        kubectl apply -f k8s/bootstrap.yaml
        ```

    * **Option B: Private Repository** <br>
        If using a private repo, first apply the access token secret, then the root app:
        
        - Create the secret (from example):
        ```bash
        cp k8s/repo-secret.yml.example k8s/repo-secret.yml
        ```
        *Edit repo-secret.yml with your token*
        <br>
        - Apply config:
        ```bash
        kubectl apply -f k8s/repo-secret.yml
        kubectl apply -f k8s/bootstrap.yaml
        ```

## Roadmap
- [x] **Home Automation:** Integrate Home Assistant for IoT orchestration
- [ ] **VPN Access:** Setup WireGuard for secure remote access
- [ ] **Observability:** Deploy Prometheus & Grafana stack for monitoring
- [ ] **Dashboard:** Configure Homepage for centralized service access
- [ ] **Media Center:** Deploy Jellyfin for local streaming
- [ ] **Disaster Recovery:** Implement encrypted off-site backups