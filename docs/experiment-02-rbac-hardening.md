# Experiment 02 — Kubernetes RBAC & Least Privilege

## Objective

Demonstrate excessive permissions in Kubernetes Role-Based Access Control (RBAC) and implement the principle of least privilege.

## Environment

- Kubernetes: K3s v1.36.5+k3s1
- Namespace: `security-lab`
- ServiceAccount: `rbac-lab-sa`
- Verification Tool: `kubectl auth can-i`

## Methodology

1. Verify the baseline permissions of the default ServiceAccount.
2. Create an intentionally overprivileged Role granting access to Pods and Secrets.
3. Bind the Role to the experimental ServiceAccount.
4. Validate permissions using Kubernetes authorization checks.
5. Remove unnecessary Secrets permissions.
6. Repeat authorization checks to verify the remediation.

## Results

| Permission | Before | After |
|---|---|---|
| List Pods | Allowed | Allowed |
| List Secrets | Allowed | Denied |
| Delete Pods | Denied | Denied |

## Security Finding

**Finding ID:** K3S-RBAC-001

**Issue:** Excessive ServiceAccount Permissions

**Description:** The experimental ServiceAccount was granted permission to list Kubernetes Secrets despite only requiring read access to Pods.

**Potential Impact:** Unnecessary access to Kubernetes Secrets could expose sensitive application configuration and credentials if the ServiceAccount were compromised.

## Remediation

The Role was modified to permit only `get` and `list` operations on Pods. Unnecessary permissions for Secrets were removed.

Automatic ServiceAccount token mounting was also disabled as an additional precaution. This setting controls token mounting and does not itself revoke RBAC permissions.

## Verification

Authorization checks confirmed that the experimental ServiceAccount retained the required Pod permissions while access to Secrets remained denied.

## Conclusion

The experiment successfully demonstrated excessive Kubernetes RBAC permissions and their remediation through the principle of least privilege.

The results validate the specific permissions tested within the `security-lab` namespace, rather than the security of the entire cluster.
