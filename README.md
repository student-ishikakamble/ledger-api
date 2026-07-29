# Ledger API - Kubernetes & Istio Security Deployment

## Overview

This project demonstrates the deployment of a secure Ledger API application on Kubernetes with Istio Service Mesh integration.

The deployment focuses on Kubernetes orchestration, service mesh security, encrypted communication, and traffic control.

## Implemented Features

* Containerized Ledger API application
* Kubernetes Deployment and Service
* Dedicated Kubernetes namespace
* Istio Service Mesh integration
* Automatic Istio sidecar injection
* Mutual TLS (mTLS) with STRICT mode
* Istio AuthorizationPolicy
* Kubernetes NetworkPolicy
* Secure service-to-service communication

---

# Architecture

```
                    Kubernetes Cluster

                           |
                           |
                    payments namespace

                           |
        -------------------------------------
        |                                   |
   ledger-api pods                    reporting pod

        |
        |
   ---------------------
   |                   |
Application        Istio Proxy
Container          (Envoy Sidecar)

```

## Technology Stack

| Technology  | Purpose                              |
| ----------- | ------------------------------------ |
| Kubernetes  | Container orchestration              |
| Istio       | Service mesh and security            |
| Envoy Proxy | Sidecar proxy for traffic management |
| Docker      | Containerization                     |
| YAML        | Kubernetes configuration             |
| mTLS        | Encrypted service communication      |

---

# Kubernetes Deployment

## Namespace Creation

Created a dedicated namespace for application deployment:

```bash
kubectl create namespace payments
```

Enabled automatic Istio sidecar injection:

```bash
kubectl label namespace payments istio-injection=enabled
```

---

# Application Deployment

Applied Kubernetes manifests:

```bash
kubectl apply -f k8s/
```

Verify deployed resources:

```bash
kubectl get all -n payments
```

Current deployment status:

```
deployment.apps/ledger-api   3/3 READY
deployment.apps/reporting    1/1 READY
```

---

# Istio Service Mesh Integration

## Sidecar Injection

Istio sidecar injection was enabled for the payments namespace.

Each Ledger API pod contains:

* Application container
* Istio Envoy proxy sidecar

Verification:

```bash
kubectl get pods -n payments
```

Expected output:

```
ledger-api-xxxxx   2/2 Running
```

The `2/2` status confirms:

* Ledger API container is running
* Istio proxy sidecar is injected successfully

---

# Security Implementation

## 1. Mutual TLS (mTLS)

Istio PeerAuthentication was configured to enforce STRICT mTLS mode.

Configuration file:

```
k8s/peer-authentication.yaml
```

Configuration:

```yaml
apiVersion: security.istio.io/v1
kind: PeerAuthentication
metadata:
  name: default
  namespace: payments
spec:
  mtls:
    mode: STRICT
```

Verification:

```bash
kubectl get peerauthentication -n payments
```

Output:

```
NAME      MODE
default   STRICT
```

Result:

✅ All service-to-service communication inside the Istio mesh is encrypted.

---

# 2. Istio Authorization Policy

Implemented Istio AuthorizationPolicy for controlling access to Ledger API.

Configuration file:

```
k8s/authorization-policy.yaml
```

Policy:

* Allows traffic only from workloads inside the payments namespace
* Blocks unauthorized service communication

Verification:

```bash
kubectl get authorizationpolicy -n payments
```

Output:

```
NAME                ACTION
ledger-api-policy   ALLOW
```

Result:

✅ Service access is controlled through Istio authorization rules.

---

# 3. Kubernetes Network Policy

Implemented Kubernetes NetworkPolicy for additional network isolation.

Configuration file:

```
k8s/network-policy.yaml
```

Policy rules:

* Applied to ledger-api pods
* Restricts incoming traffic
* Allows only traffic from payments namespace

Verification:

```bash
kubectl describe networkpolicy ledger-api-network-policy -n payments
```

Result:

✅ Kubernetes network-level traffic restrictions are enabled.

---

# Verification Results

## Pods

Command:

```bash
kubectl get pods -n payments
```

Result:

```
ledger-api-d79c9959c-ccxzh   2/2 Running
ledger-api-d79c9959c-ghq6c   2/2 Running
ledger-api-d79c9959c-nfmns   2/2 Running
```

---

## mTLS Verification

Command:

```bash
kubectl get peerauthentication -n payments
```

Result:

```
NAME      MODE
default   STRICT
```

---

## Authorization Verification

Command:

```bash
kubectl get authorizationpolicy -n payments
```

Result:

```
NAME                ACTION
ledger-api-policy   ALLOW
```

---

## NetworkPolicy Verification

Command:

```bash
kubectl get networkpolicy -n payments
```

Result:

```
NAME                        POD-SELECTOR
ledger-api-network-policy   app=ledger-api
```

---

# Useful Kubernetes Commands

## Check Pods

```bash
kubectl get pods -n payments
```

## Check Services

```bash
kubectl get svc -n payments
```

## Check Deployments

```bash
kubectl get deployments -n payments
```

## Check Istio mTLS

```bash
kubectl get peerauthentication -n payments
```

## Check Authorization Policies

```bash
kubectl get authorizationpolicy -n payments
```

## Check Network Policies

```bash
kubectl get networkpolicy -n payments
```

---

# Security Approach

This implementation follows a defense-in-depth security model:

1. Kubernetes namespace isolation
2. Istio service mesh security
3. Mutual TLS encryption
4. Service-level authorization
5. Kubernetes network traffic restriction

---

# Final Assignment Status

| Requirement           | Status     |
| --------------------- | ---------- |
| Kubernetes Deployment | ✅ Complete |
| Service Configuration | ✅ Complete |
| Istio Installation    | ✅ Complete |
| Sidecar Injection     | ✅ Complete |
| mTLS STRICT Mode      | ✅ Complete |
| AuthorizationPolicy   | ✅ Complete |
| NetworkPolicy         | ✅ Complete |
| Security Verification | ✅ Complete |

---

# Conclusion

The Ledger API application has been successfully deployed on Kubernetes with Istio-based security controls.

The deployment provides:

* Secure encrypted communication
* Controlled service access
* Network isolation
* Production-style Kubernetes security practices

Task completed successfully.
