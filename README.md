# Homelab Infrastructure Map 
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)

## Architecture
```
[ ISP / Internet ]
      │
      ▼
[ Router ] (192.168.100.1)
      │
      ├── (Cable) ─── [ Sony TV ] (Client for Jellyfin)
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

## Hardware Specs
- Node: Lenovo ThinkPad L480
- CPU: i5-8250U (4c/8t)
- RAM: 8GB
- OS: Ubuntu Server 24.04 LTS