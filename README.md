# Homelab 
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)

## Repo Structure
```
homelab-infrastructure/
├── ansible/                         # Ansible configuration & automation
│   ├── inventory/                  
│   │   ├── group_vars/              # Group-level variables
│   │   │   └── lab_servers.yml      # Shared variables for lab servers
│   │   └── hosts                    # Inventory file
│   │
│   ├── playbooks/                  # Standalone playbooks
│   │   ├── bootstrap.yml            # Initial server bootstrap
│   │   └── ping.yml                 # Connectivity test playbook
│   │
│   ├── roles/                      # Ansible roles
│   │   ├── common/                  # Base system configuration
│   │   │   ├── tasks/
│   │   │   │   └── main.yml
│   │   │   └── handlers/
│   │   │       └── main.yml
│   │   │
│   │   ├── docker/                  # Docker installation and config
│   │   │   ├── tasks/
│   │   │   │   └── main.yml
│   │   │   └── handlers/
│   │   │       └── main.yml
│   │   │
│   │   ├── k3s/                     # Kubernetes (k3s) setup
│   │   │   └── tasks/
│   │   │       └── main.yml
│   │   │
│   │   └── security/                # Server security & hardening
│   │       ├── tasks/
│   │       │   └── main.yml
│   │       ├── handlers/
│   │       │   └── main.yml
│   │       └── templates/
│   │           └── sshd_config.j2   # SSH configuration template
│   │
│   ├── ansible.cfg                  # Global Ansible configuration
│   └── site.yml                     # Main entry-point playbook
│
├── docs/                            # Project documentation
│   └── k3s-architecture.md          # k3s cluster architecture
│
└── README.md                        # Project overview and usage
```

## Homelab Infrastructure Map 

```
[ ISP / Internet ]
      │
      ▼
[ Router ] (192.168.100.1)
      │
      ├── (Cable) ─── [ Sony BRAVIA 5 ] (Client for Jellyfin)
      │
      ├── (Cable) ─── [ ThinkPad L480 ] (Server - 192.168.100.30)
      │                 │
      │                 ├── Docker: Jellyfin
      │                 ├── Docker: AdGuard Home 
      │                 ├── Docker: Home Assistant 
      │                 └── K3s Cluster
      │
      └── (Wi-Fi) ──── Macbook M3 (Ansible Control Node)
```

## Server Hardware Specs
- Node: Lenovo ThinkPad L480
- CPU: i5-8250U (4c/8t)
- RAM: 8GB
- OS: Ubuntu Server 24.04 LTS