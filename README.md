# Financial Security Standards

A comprehensive collection of Kubernetes security policies using Kyverno to enforce financial industry security standards and best practices.

## Overview

This repository contains security policies organized into different categories to ensure comprehensive security coverage for Kubernetes environments in financial services. All policies are implemented using Kyverno ClusterPolicies and follow industry security standards including Pod Security Standards, EKS Best Practices, and RBAC best practices.

## Policy Categories

### 1. Identity and Access Management (`identity-and-access-management/`)

Policies focused on identity verification, access control, and authentication mechanisms.

**Policies:**
- `check-cluster-endpoint.yaml` - Validates cluster endpoint configuration
- `check-instance-profile-access.yaml` - Ensures proper EC2 instance profile access controls
- `require-aws-node-irsa.yaml` - Enforces IAM Roles for Service Accounts (IRSA) usage
- `require-run-as-non-root-user.yaml` - Mandates containers run as non-root users
- `restrict-automount-sa-token.yaml` - Controls automatic mounting of service account tokens
- `restrict-binding-system-groups.yaml` - Prevents binding to system groups
- `restrict-wildcard-resources.yaml` - Restricts use of wildcard resources in RBAC
- `restrict-wildcard-verbs.yaml` - Restricts use of wildcard verbs in RBAC

### 2. Mandatory Security Control Baseline (`mandatory-security-control-baseline/`)

Essential security controls that form the baseline security requirements for all workloads.

**Policies:**
- `disallow-capabilities.yaml` - Prevents dangerous Linux capabilities
- `disallow-host-namespaces.yaml` - Blocks access to host namespaces (PID, IPC, Network)
- `disallow-host-path.yaml` - Prevents mounting of host paths
- `disallow-host-ports.yaml` - Blocks binding to host ports
- `disallow-host-process.yaml` - Prevents host process containers
- `disallow-privileged-containers.yaml` - Blocks privileged container execution
- `disallow-proc-mount.yaml` - Restricts proc mount types
- `disallow-selinux.yaml` - Controls SELinux configuration
- `restrict-apparmor-profiles.yaml` - Enforces AppArmor profile restrictions
- `restrict-seccomp.yaml` - Mandates proper seccomp profiles
- `restrict-sysctls.yaml` - Controls allowed sysctls

### 3. Role-Based Access Control (`role-based-access-control/`)

Policies governing RBAC permissions and access patterns.

**Policies:**
- `disable-automount-sa-token.yaml` - Disables automatic service account token mounting
- `restrict-automount-sa-token.yaml` - Controls when service account tokens can be mounted
- `restrict-binding-system-groups.yaml` - Prevents binding to system groups
- `restrict-clusterrole-nodesproxy.yaml` - Restricts nodes/proxy ClusterRole usage
- `restrict-escalation-verbs-roles.yaml` - Prevents privilege escalation verbs (bind, escalate, impersonate)
- `restrict-wildcard-resources.yaml` - Limits wildcard resource usage in roles

### 4. Secrets Management (`secrets-management/`)

Policies for secure handling and management of sensitive data.

**Policies:**
- `disallow-all-secrets.yaml` - Comprehensive restrictions on secret usage
- `disallow-secrets-from-env-vars.yaml` - Prevents secrets exposure through environment variables

### 5. Supply Chain Security (`supply-chain-security/`)

Policies ensuring the integrity and security of container images and the software supply chain.

**Policies:**
- `allowed-base-images.yaml` - Defines approved base images
- `check-immutable-tags-ecr.yaml` - Validates immutable tags in ECR
- `require-base-image.yaml` - Mandates use of approved base images
- `restrict-image-registries.yaml` - Limits allowed container registries

## Policy Structure

All policies follow a consistent structure:
- **Metadata**: Policy name, annotations, and categorization
- **Validation Action**: Set to `audit` mode for monitoring
- **Background**: Enabled for existing resource validation
- **Rules**: Specific validation logic and constraints

## Policy Annotations

Each policy includes standardized annotations:
- `policies.kyverno.io/title` - Human-readable policy title
- `policies.kyverno.io/category` - Policy classification
- `policies.kyverno.io/severity` - Risk level (low, medium, high)
- `policies.kyverno.io/subject` - Kubernetes resources affected
- `policies.kyverno.io/description` - Detailed policy explanation

## Compliance Standards

These policies help achieve compliance with:
- **Pod Security Standards** (Baseline and Restricted)
- **EKS Best Practices**
- **RBAC Best Practices**
- **Financial Services Security Requirements**
- **CIS Kubernetes Benchmark**

## Usage

1. **Apply individual policies:**
   ```bash
   kubectl apply -f identity-and-access-management/require-run-as-non-root-user.yaml
   ```

2. **Apply entire category:**
   ```bash
   kubectl apply -f mandatory-security-control-baseline/
   ```

3. **Apply all policies:**
   ```bash
   kubectl apply -f .
   ```

## Policy Validation Modes

All policies are currently set to `audit` mode for monitoring and compliance reporting. To enforce policies, change `validationFailureAction` from `audit` to `enforce` in the policy specifications.

## Monitoring and Reporting

Use the following commands to monitor policy violations:

```bash
# View policy violations
kubectl get events --field-selector type=Warning

# Check specific policy status
kubectl get cpol <policy-name> -o yaml

# View policy reports (if PolicyReports are enabled)
kubectl get polr -A
```

## Customization

Many policies require customization for your specific environment:
- Update allowed image registries in supply chain policies
- Modify RBAC restrictions based on your access patterns
- Adjust security baselines per your risk tolerance

## Contributing

When adding new policies:
1. Follow the existing naming conventions
2. Include proper annotations and metadata
3. Set appropriate severity levels
4. Provide clear descriptions and remediation guidance
5. Test in audit mode before enforcing

## Security Considerations

- Regularly review and update policies
- Monitor policy violations and adjust as needed
- Test policy changes in non-production environments
- Maintain documentation for policy exceptions
- Ensure policies align with organizational security standards