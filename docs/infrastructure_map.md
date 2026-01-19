# Homelab Infrastructure Map

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