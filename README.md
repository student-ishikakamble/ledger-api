# Ledger API Security & DevOps Assessment

## Overview

This repository contains my submission for the **Dodo Payments Security & DevOps Engineer Technical Assessment**.

The objective of this assessment was to transform an intentionally vulnerable application into a production-ready deployment by implementing Kubernetes security hardening, secure CI/CD practices, Zero Trust networking using Istio, and an authorized security assessment.

---

# Assessment Progress

| Task | Status |
|------|--------|
| ✅ Task 1 – Deploy & Harden the Workload | Completed |
| ✅ Task 2 – Secure CI/CD Pipeline & Supply Chain | Completed |
| ✅ Task 3 – Service Mesh & Zero Trust (Istio) | Completed |
| ✅ Task 4 – Reconnaissance & Penetration Testing | Completed |

---

# Repository Structure

```
.
├── .github/
│   └── workflows/
├── app/
├── deploy/
├── docs/
├── k8s/
├── policies/
├── security/
├── task4/
│   ├── recon/
│   ├── report/
│   └── screenshots/
├── .gitleaks.toml
└── README.md
```

---

# Architecture

```
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├── Semgrep
    ├── Trivy
    ├── Gitleaks
    └── Cosign
    │
    ▼
ArgoCD (GitOps)
    │
    ▼
Kubernetes Cluster
    │
┌─────────────────────────────────────────┐
│ payments Namespace                      │
│                                         │
│  ┌───────────────┐     ┌──────────────┐ │
│  │ Ledger API    │────▶│ Reporting    │ │
│  │ + Envoy       │     │ Service      │ │
│  └───────────────┘     └──────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

---

# Task 1 – Deploy & Harden the Workload

## Objective

Deploy the Ledger API on Kubernetes and apply production-grade security controls.

## Implemented

- Kubernetes Deployment
- Kubernetes Service
- Dedicated Namespace
- ConfigMap
- Ingress
- Dedicated ServiceAccount
- RBAC (Least Privilege)
- SecurityContext
  - Non-root container
  - Read-only root filesystem
  - Drop all Linux capabilities
  - seccomp RuntimeDefault
- Resource Requests & Limits
- Liveness Probe
- Readiness Probe
- Sealed Secrets
- Kyverno / OPA Admission Policies

## Security Decisions

- Secrets removed from Git and encrypted using Sealed Secrets.
- Least-privilege RBAC implemented.
- SecurityContext applied to every workload.
- Admission policies prevent insecure deployments.

---

# Task 2 – Secure CI/CD Pipeline & Supply Chain

## Objective

Build a secure DevSecOps pipeline.

## Implemented

- GitHub Actions
- Semgrep (SAST)
- Trivy Dependency Scan
- Trivy Image Scan
- Gitleaks Secret Scan
- Cosign Image Signing
- ArgoCD GitOps Deployment

## Security Gates

| Tool | Purpose | Fail Policy |
|------|----------|------------|
| Gitleaks | Secret Detection | Block build |
| Semgrep | Static Code Analysis | Block on Critical findings |
| Trivy | Dependency & Image Scan | Block on Critical CVEs |
| Cosign | Image Signing | Only signed images are deployed |

---

# Task 3 – Service Mesh & Zero Trust

## Objective

Secure workload communication using Istio.

## Implemented

- Istio Installation
- Automatic Sidecar Injection
- STRICT mTLS
- AuthorizationPolicy
- NetworkPolicy

## Certificate Management

Istiod automatically issues short-lived SPIFFE workload certificates.

Certificates are rotated automatically.

The trust root is managed by Istiod.

## Defense in Depth

| Layer | Purpose |
|--------|----------|
| RBAC | Least Privilege |
| SecurityContext | Hardened Containers |
| NetworkPolicy | Kubernetes Network Isolation |
| Istio mTLS | Encrypted Communication |
| AuthorizationPolicy | Identity-based Access Control |
| Sealed Secrets | Secret Protection |

---

# Task 4 – Reconnaissance & Penetration Testing

## Part A – Passive Reconnaissance

Performed passive reconnaissance using publicly available information only.

### Activities

- Certificate Transparency Logs (crt.sh)
- Passive DNS Enumeration
- Subdomain Enumeration
- Attack Surface Analysis

### Deliverables

```
task4/recon/subdomains.txt
task4/report/recon-report.md
task4/screenshots/task4-crtsh.png
```

---

## Part B – Authorized Penetration Testing

Active testing was performed **only against the provided vulnerable application**.

### Findings

- Sensitive Transaction Data Exposure
- Server-Side Request Forgery (SSRF) behavior via `/fetch`
- Unsafe YAML Loading observation

Each finding includes:

- CVSS v3.1 Score
- Severity
- Reproduction Steps
- Impact
- Remediation

### Deliverables

```
task4/report/pentest-report.md
task4/screenshots/pentest/transactions.png
task4/screenshots/pentest/ssrf.png
```

---

# PCI DSS Alignment

This implementation aligns with PCI DSS security principles:

- Least Privilege Access
- Secret Management
- Secure Container Configuration
- Network Segmentation
- Encrypted Service Communication
- Secure CI/CD Pipeline
- Defense in Depth

---

# Verification Commands

```bash
kubectl get pods -n payments

kubectl get svc -n payments

kubectl get deployments -n payments

kubectl get peerauthentication -n payments

kubectl get authorizationpolicy -n payments

kubectl get networkpolicy -n payments
```

---

# Technologies Used

| Technology | Purpose |
|------------|----------|
| Docker | Containerization |
| Kubernetes | Container Orchestration |
| GitHub Actions | CI/CD |
| Istio | Service Mesh |
| ArgoCD | GitOps |
| Semgrep | Static Application Security Testing |
| Trivy | Dependency & Image Vulnerability Scanning |
| Gitleaks | Secret Detection |
| Cosign | Image Signing |
| Kyverno / OPA | Admission Policy Enforcement |
| Sealed Secrets | Secret Encryption |

---

# How to Run

### Clone Repository

```bash
git clone https://github.com/student-ishikakamble/ledger-api.git

cd ledger-api
```

### Deploy Kubernetes Resources

```bash
kubectl apply -f k8s/
```

### Verify Deployment

```bash
kubectl get all -n payments
```

### Verify Istio Security

```bash
kubectl get peerauthentication -n payments

kubectl get authorizationpolicy -n payments

kubectl get networkpolicy -n payments
```

---

# Evidence

Repository includes screenshots for:

- Kubernetes Deployment
- GitHub Actions Pipeline
- Istio Sidecar Injection
- mTLS Verification
- AuthorizationPolicy
- NetworkPolicy
- Task 4 Passive Reconnaissance
- Task 4 Penetration Testing

---

# Conclusion

This project demonstrates a defense-in-depth approach by combining Kubernetes workload hardening, secure software delivery, Zero Trust networking, and structured offensive security testing.

The implementation focuses on automation, least privilege, encrypted communication, policy enforcement, and reproducible security practices while aligning with the objectives of the Dodo Payments Security & DevOps Engineer Technical Assessment.
