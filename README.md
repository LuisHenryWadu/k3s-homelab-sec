# K3s Security Homelab

**A hands-on Kubernetes security project focused on identifying misconfigurations, implementing security hardening, and validating improvements through security assessments.**

## Overview

This project demonstrates Kubernetes security assessment and hardening using a lightweight K3s cluster deployed on a self-hosted Acer Aspire 5 laptop running Ubuntu Server.

The lab focuses on practical security engineering, including container security, Kubernetes Role-Based Access Control (RBAC), network isolation, and cluster configuration assessment.

Security experiments are conducted in a dedicated `security-lab` namespace to minimize interference with existing workloads.

## Technology Stack

| Category | Technology |
|---|---|
| Operating System | Ubuntu Server 26.04.1 LTS |
| Hardware | Acer Aspire 5 |
| Container Orchestration | K3s (Kubernetes) |
| Container Runtime | containerd |
| Security Assessment | Kubescape v4.0.15 |
| Security Framework | NSA/CISA Kubernetes Hardening Guidance |
| Networking | Kubernetes NetworkPolicy |
| Access Management | Kubernetes RBAC |
| Configuration | Kubernetes YAML manifests |
| Version Control | Git & GitHub |

## Security Assessment

An initial cluster-wide security assessment was performed using Kubescape against the NSA security framework.

### Initial Cluster Baseline

| Metric | Result |
|---|---|
| Compliance Score | 54.04% |
| Evaluated Controls | 24 of 26 |
| Passed Controls | 7 |
| Failed Controls | 14 |
| Action Required | 5 |
| Scan Coverage | 83% |

The scan identified several areas requiring security improvements, including missing container resource limits, excessive privileges, insufficient network restrictions, and incomplete container hardening.

A separate verification also confirmed that K3s Secrets encryption at rest was disabled.

**Note:** These findings describe the initial cluster-wide assessment. Improvements made to experimental manifests do not imply that the entire cluster has been hardened.

## Security Experiments

### Experiment 01 — Container Security Hardening

**Objective:** Identify and remediate insecure container configurations.

An intentionally misconfigured BusyBox Pod was created and analyzed using Kubescape.

The workload was subsequently hardened using Kubernetes Security Context settings.

Hardening measures included:

- Running containers as a non-root user
- Disabling privilege escalation
- Dropping all Linux capabilities
- Enabling the RuntimeDefault seccomp profile
- Enforcing a read-only root filesystem

**Assessment Results**

| Configuration | NSA Compliance |
|---|---|
| Intentionally Insecure Pod | 75% |
| Hardened Pod | 95% |
| Hardened Pod + NetworkPolicy | 100% |

Runtime verification confirmed that the hardened container operated with UID 10001, zero effective capabilities, seccomp filtering, and a read-only root filesystem.

The 100% result applies only to the assessed experimental manifests, not the entire K3s cluster.

[Read Experiment 01](docs/experiment-01-container-hardening.md)

### Experiment 02 — RBAC & Least Privilege

**Objective:** Demonstrate excessive Kubernetes permissions and implement least-privilege access control.

An experimental ServiceAccount was initially granted unnecessary read access to Kubernetes Secrets.

The Role was then modified to retain only the permissions required for reading Pods.

**Authorization Test Results**

| Permission | Before | After |
|---|---|---|
| List Pods | Allowed | Allowed |
| List Secrets | Allowed | Denied |
| Delete Pods | Denied | Denied |

Authorization checks were performed using `kubectl auth can-i`.

[Read Experiment 02](docs/experiment-02-rbac-hardening.md)

### Network Isolation Validation

A default-deny NetworkPolicy was configured in the `security-lab` namespace.

DNS connectivity was tested under three conditions:

1. Default-deny policy active — DNS blocked
2. Temporary DNS allowance — DNS resolved successfully
3. Temporary allowance removed — DNS blocked again

The results verified DNS egress policy enforcement for the experimental Pod.

[Read Network Isolation Tests](docs/network-isolation.md)

## Repository Structure

```text
k3s-security-homelab/
├── README.md
├── .gitignore
├── k8s/
│   ├── base/
│   ├── misconfigured/
│   ├── hardened/
│   └── tests/
├── docs/
│   ├── experiment-01-container-hardening.md
│   ├── experiment-02-rbac-hardening.md
│   └── network-isolation.md
└── reports/
    └── (local reports excluded from Git)
```

## Security Considerations

This repository contains intentionally insecure Kubernetes configurations created exclusively for educational security testing.

- Insecure manifests must not be deployed to production environments.
- Experimental resources should be deployed only in controlled environments.
- Raw assessment reports, credentials, and kubeconfig files must not be committed.
- Security testing is restricted to authorized infrastructure.

## Future Improvements

- Kubernetes Secrets management and encryption at rest
- Cluster-level security hardening
- Additional RBAC and NetworkPolicy assessments
- Container image vulnerability scanning with Trivy
- Automated configuration scanning through CI/CD

## Project Scope

This project is an independently developed, self-hosted Kubernetes security learning environment.

The primary goal is to build practical experience in Kubernetes administration, security assessment, infrastructure hardening, and technical documentation.
