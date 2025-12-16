## K3s Internal Architecture (Single-Node Cluster)

```
[ Physical Server (ThinkPad L480 / Ubuntu 24.04) ]
    │
    ├── [ K3s Server Process ] (The Brain)
    │     │
    │     ├── API Server       (Entry point for kubectl/Ansible)
    │     ├── Scheduler        (Decides where to put pods)
    │     ├── Controller Mgr   (Ensures state matches desire)
    │     └── SQLite           (Database - stores cluster state)
    │
    ├── [ Containerd ] (Container Runtime)
    │     │ The engine that actually runs the containers
    │     │ (Replaces Docker Engine for K8s)
    │
    ├── [ Kubelet ] (The Agent)
    │     │ Talks to API Server and tells Containerd what to do
    │
    └── [ Kube-Proxy ] (Network Glue)
        │ Manages network rules (iptables) so Services work
```

## Ingress Request Flow in a K3s Cluster

```
[ User / Device ]
      │
      │ jellyfin.home
      ▼
[ DNS / Hosts File ] ─── returns IP ──▶ 192.168.100.xx
      │
      │ HTTP Request (GET /)
      ▼
[ Server Node (192.168.100.xx) ]
      │
      ├── [ Traefik ] (Ingress Controller)
      │      │ Listens on ports 80/443
      │      │ Matches domain "jellyfin.home"
      │      ▼
      ├── [ Service: Jellyfin ] (Internal Load Balancer)
      │      │ Virtual IP inside cluster
      │      │ Finds healthy pods
      │      ▼
      └── [ Pod: Jellyfin ] (Container)
             │ Runs the actual app
             │ Mounts volume /media/movies
```