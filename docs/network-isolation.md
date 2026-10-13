# NetworkPolicy Validation

## Objective
Verify DNS egress isolation in the security-lab namespace.

## Environment
- Kubernetes: K3s v1.36.5+k3s1
- Node: serverluis
- Namespace: security-lab

## Test Results

| Test | Expected | Actual | Status |
|---|---|---|---|
| Default-deny | DNS blocked | DNS failed | PASS |
| Allow DNS | DNS resolved | 10.43.0.1 | PASS |
| Remove allowance | DNS blocked | DNS failed | PASS |

## Conclusion
The NetworkPolicy enforcement test confirmed that
DNS egress was blocked by default, permitted through
an explicit allow rule, and blocked again when the
allow rule was removed.

Further ingress and inter-pod connectivity testing
is required to validate broader network isolation.
