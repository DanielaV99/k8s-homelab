# K8s Homelab

A self-hosted Kubernetes cluster running on 2 physical laptops, providing a platform for DNS-based ad blocking, monitoring, and custom applications.

## 🎯 Overview

This repository contains the configuration and documentation for a 3-node Kubernetes homelab cluster designed to run on minimal hardware. The cluster features:

- **3 Kubernetes nodes** across 2 physical machines (ASUS VivoBook + Dell Precision 5540)
- **AdGuard Home** for network-wide DNS-based ad blocking and tracker protection
- **Grafana** with dashboards for fitness data visualization and monitoring
- **Garmin Database** integration with automated data collection and web interface
- **Cloudflare Tunnel** for secure external access
- **Calico CNI** with VXLAN encapsulation for pod networking

## 🖥️ Hardware Architecture

| Physical Machine | Kubernetes Nodes | Role | Specs |
|-----------------|----------------|------|-------|
| **ASUS VivoBook** | `master` (VM) + `asuslpt` | Control Plane + Worker | Intel i3-2.2GHz, 8GB RAM, 512GB SSD |
| **Dell Precision 5540** | `precision5540` | Worker | Intel i7-9850H, 16GB RAM, NVMe SSD |

### Cluster Details

- **Kubernetes Version:** v1.34.2
- **CNI:** Calico (VXLAN encapsulation, BGP disabled)
- **Network:** 192.168.1.0/24
- **External Access:** Cloudflare Tunnel

See [architecture.md](docs/architecture.md) for detailed hardware and node specifications.

## 📁 Repository Structure

```
k8s-homelab/
├── apps/                          # Kubernetes application manifests
│   └── grafana/                   # Grafana stack with Garmin integration
│       ├── dashboards/            # Grafana JSON dashboards
│       ├── garmin-db-pvc.yaml     # Persistent volume for Garmin data
│       ├── garmindb-configmap.yaml# Garmin database configuration
│       ├── garmindb-cronjob.yaml  # Automated Garmin data collection
│       ├── grafana-values.yaml    # Helm values for Grafana
│       └── sqlite-web.yaml        # SQLite web interface for database
├── docs/                          # Documentation
│   ├── architecture.md            # Cluster architecture overview
│   ├── setup/                     # Setup guides
│   │   └── adguard-home.md       # AdGuard Home installation guide
│   └── troubleshooting/           # Troubleshooting guides
│       └── wifi-node-debugging.md # WiFi node networking issues
└── README.md                      # This file
```

## 🚀 Quick Start

### Prerequisites

- Two machines with Ubuntu Server 24.04 (or compatible Linux)
- `kubectl` and `helm` installed
- Access to router configuration
- Network connectivity between machines

### Basic Setup

1. **Initialize Kubernetes cluster** on the master node
2. **Join worker nodes** to the cluster
3. **Install CNI plugin** (Calico)
4. **Deploy applications** using provided manifests

For detailed setup instructions, see [architecture.md](docs/architecture.md).

## 📦 Deployed Applications

### AdGuard Home
Network-wide DNS-based ad blocking and tracker protection.

- **Status:** Deployed
- **Documentation:** [adguard-home.md](docs/setup/adguard-home.md)
- **Features:** DNS filtering, query logging, dashboard

### Grafana + Garmin Integration
Monitoring and fitness data visualization.

- **Status:** Deployed
- **Components:**
  - Grafana dashboard for monitoring
  - Garmin data collection via cronjob
  - SQLite database for fitness data
  - Web interface for database browsing

## 📚 Documentation

### Architecture
Comprehensive overview of the cluster design, hardware, networking, and component relationships.
- [architecture.md](docs/architecture.md)

### Setup Guides
Step-by-step instructions for deploying specific components.
- [AdGuard Home Setup](docs/setup/adguard-home.md)

### Troubleshooting
Solutions for common issues and debugging strategies.
- [WiFi Node Debugging](docs/troubleshooting/wifi-node-debugging.md)

## 🌐 Network Configuration

- **Cluster Network:** 192.168.1.0/24
- **Control Plane:** 192.168.1.50 (master VM)
- **Worker Nodes:** 192.168.1.104 (ASUS), 192.168.1.105 (Dell)
- **External Access:** Cloudflare Tunnel

## 🔐 Security Considerations

- Use Cloudflare Tunnel for external access instead of exposing services directly
- Configure network policies for pod-to-pod communication
- Secure sensitive data using Kubernetes Secrets
- Regularly update cluster and application components
