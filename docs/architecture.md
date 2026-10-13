# Homelab Architecture

## Overview

This project runs on a self-hosted Acer Aspire 5 laptop configured as a single-node K3s Kubernetes cluster.

The infrastructure supports existing IoT monitoring workloads alongside an isolated environment for Kubernetes security experiments.

## Infrastructure

| Component | Specification |
|---|---|
| Host | Acer Aspire 5 |
| CPU | Intel Core i3-1005G1 |
| RAM | 8 GB |
| Operating System | Ubuntu Server 26.04.1 LTS |
| Kubernetes | K3s v1.36.5+k3s1 |
| Container Runtime | containerd |
| Cluster Type | Single-node control plane |

## Kubernetes Architecture

```text
Acer Aspire 5
└── Ubuntu Server
    └── K3s Kubernetes Cluster
        ├── kube-system
        │   ├── CoreDNS
        │   ├── Traefik
        │   └── Metrics Server
        │
        ├── default
        │   ├── Grafana
        │   ├── InfluxDB
        │   └── Mosquitto
        │
        └── security-lab
            ├── ResourceQuota
            ├── Default-Deny NetworkPolicy
            └── Temporary Security Experiments
```

## Security Lab Design

Security experiments are performed within a dedicated Kubernetes namespace named `security-lab`.

The namespace includes resource quotas and a default-deny NetworkPolicy to reduce unintended communication and resource consumption.

The security lab shares the underlying K3s node with existing workloads. Namespace isolation does not provide the same security boundary as a separate virtual machine or physical host.

## Security Tools

- **Kubescape:** Kubernetes security configuration assessment against the NSA/CISA framework
- **kubectl:** Cluster administration, RBAC authorization testing, and runtime verification
- **Git:** Version control for manifests and experiment documentation

## Design Considerations

The project prioritizes lightweight security experiments due to limited hardware resources.

Existing IoT workloads are kept operational while security experiments are conducted using dedicated Kubernetes resources.

Intentionally insecure configurations are maintained as local manifest examples and are not intended for production deployment.
