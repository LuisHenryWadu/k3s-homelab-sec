# Experiment 01 — Kubernetes Container Security Hardening

## Objective

Evaluate and improve container security configurations in a K3s environment using Kubescape and Kubernetes Security Context.

## Environment

- Host: Acer Aspire 5
- OS: Ubuntu Server 26.04.1 LTS
- Kubernetes: K3s v1.36.5+k3s1
- Security Scanner: Kubescape v4.0.15
- Security Framework: NSA/CISA
- Namespace: `security-lab`

## Methodology

1. Create an intentionally misconfigured BusyBox Pod manifest.
2. Perform a static security assessment using Kubescape.
3. Identify failed security controls.
4. Create a hardened Pod manifest with improved security settings.
5. Repeat the static assessment.
6. Deploy the hardened Pod to the isolated namespace.
7. Verify the effective security settings at runtime.

## Results

| Security Control | Before | After |
|---|---|---|
| Non-root containers | Failed | Passed |
| Privilege escalation | Failed | Passed |
| Linux hardening | Failed | Passed |
| Immutable container filesystem | Failed | Passed |
| Ingress and Egress blocked | Failed | Failed |

**Compliance Score: 75% → 95%**

The remaining NetworkPolicy finding requires a contextual assessment including the network policies applied to the namespace.

## Runtime Verification

| Test | Observed Result | Status |
|---|---|---|
| Non-root execution | UID 10001 | PASS |
| Privilege escalation protection | NoNewPrivs = 1 | PASS |
| Linux capabilities | CapEff = 0000000000000000 | PASS |
| Seccomp | Mode 2 (filtering) | PASS |
| Read-only root filesystem | Root mount contains `ro` | PASS |

## Conclusion

The experiment demonstrated that implementing Kubernetes Security Context settings improved the security posture of the container manifest. Kubescape compliance increased from 75% to 95%, and runtime verification confirmed the effectiveness of the applied container hardening configurations.

The results apply to this experimental workload and do not represent the security posture of the entire K3s cluster.

## Limitations

- The insecure Pod was evaluated through static scanning only.
- The hardened Pod was evaluated through both static scanning and runtime testing.
- The remaining NetworkPolicy finding requires additional contextual assessment.
- No container image vulnerability scan was performed as part of this experiment.

## Assessment Results

| Assessment | Passed | Failed | Score |
|---|---:|---:|---:|
| Insecure Pod | 15 | 5 | 75% |
| Hardened Pod | 19 | 1 | 95% |
| Hardened Pod + NetworkPolicy | 20 | 0 | 100% |

## NetworkPolicy Assessment

The remaining network isolation finding was resolved
during the contextual assessment by including the
existing default-deny NetworkPolicy manifest.

The experiment achieved 100% NSA framework compliance
for the scanned manifests.

Runtime verification also confirmed:
- Non-root execution (UID 10001)
- NoNewPrivs enabled
- Zero effective Linux capabilities
- Seccomp filtering enabled
- Read-only root filesystem

Note: These results do not represent cluster-wide
compliance or guarantee the absence of vulnerabilities.
