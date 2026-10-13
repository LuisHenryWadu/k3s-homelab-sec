# Kubernetes Security Baseline Assessment

## Objective

Assess the initial security posture of a self-hosted K3s cluster before implementing security hardening measures.

## Assessment Environment

- Scanner: Kubescape v4.0.15
- Framework: NSA/CISA Kubernetes Hardening Guidance
- Kubernetes: K3s v1.36.5+k3s1
- Assessment Date: October 13, 2026

## Initial Assessment Results

| Metric | Result |
|---|---|
| Overall Compliance | 54.04% |
| Passed Controls | 7 |
| Failed Controls | 14 |
| Action Required | 5 |
| Reported Scan Coverage | 83% |
| Critical Failed Checks | 0 |
| High-Severity Failed Checks | 24 |
| Medium-Severity Failed Checks | 61 |
| Low-Severity Failed Checks | 7 |

Severity counts represent resource-level control failures, not distinct CVEs or confirmed exploitable vulnerabilities.

## Key Security Findings

### K3S-001 — Missing Resource Limits

Grafana, InfluxDB, and Mosquitto deployments were confirmed to have no CPU or memory limits configured.

**Potential Impact:** Uncontrolled resource consumption could affect application availability.

**Recommendation:** Establish appropriate requests and limits based on observed application resource usage.

### K3S-002 — Secrets Encryption Disabled

The command `k3s secrets-encrypt status` confirmed that Kubernetes Secrets encryption at rest was disabled.

**Potential Impact:** Secrets stored in the Kubernetes datastore lack the additional protection provided by K3s Secrets encryption at rest.

**Recommendation:** Evaluate enabling Secrets encryption after completing datastore backup and maintenance planning.

### K3S-003 — Container Security Misconfigurations

Kubescape identified workloads failing checks related to non-root execution, privilege escalation, and Linux hardening.

**Recommendation:** Review workload security contexts and apply least-privilege settings where compatible.

### K3S-004 — Insufficient Network Isolation

The initial assessment identified multiple resources failing network isolation controls.

**Recommendation:** Review network communication requirements and implement appropriate NetworkPolicies.

## Assessment Limitations

Two Kubelet-related controls could not be evaluated because required host information was unavailable to the CLI scanner.

Some controls require manual verification or additional tooling.

The compliance score represents assessment results against a specific framework and is not a guarantee of overall cluster security.

## Remediation Strategy

Security improvements are first tested against isolated experimental workloads.

Validated changes are documented using before-and-after assessments and runtime verification.

Changes to existing workloads and cluster-wide settings are deferred until operational risks are understood.

## Conclusion

The initial assessment identified several security hardening opportunities within the K3s environment.

The baseline provides a reference for future improvements while preserving the distinction between experimental workload hardening and cluster-wide remediation.
