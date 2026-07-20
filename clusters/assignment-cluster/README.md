# Task 5: FluxCD Image Automation & GitOps Deployment Pipeline

This directory contains the GitOps manifests and architecture documentation for implementing automated deployment updates using **FluxCD Image Automation**.

When a CI pipeline pushes a new Docker image to the container registry, FluxCD automatically detects the new image tag, writes the updated tag back to the Git repository manifest source of truth, and reconciles the deployment inside the Kubernetes cluster.

---

## 🏗 GitOps Architecture

```text
 ┌─────────────┐                      ┌─────────────────────────┐
 │ CI Pipeline │ ─ (Pushes Image) ──> │ Container Registry      │
 └─────────────┘                      └────────────┬────────────┘
                                                   │ (Scans Tags)
                                                   ▼
┌─────────────────────────────────────────────────────────────────┐
│ Kubernetes Cluster (FluxCD Controllers)                         │
│                                                                 │
│  1. ImageRepository ──> Detects new image tag                   │
│  2. ImagePolicy      ──> Filters tags (SemVer or Regex)         │
│  3. ImageUpdateAutomation ──> Commits new tag to Git repo       │
│                                                                 │
│  4. GitRepository / Kustomization ──> Reconciles cluster state  │
└─────────────────────────────────────────────────────────────────┘
