# Platform-engineering-lab

GitOps Driven Kubernetes Platform

*This repository demonstrates how a small organisation can operator Kubernetes using GitOps as a single source of truth*

The focus of this project is platform engineering fundamentals:

- Clear ownership of boundaries
- Git-Based control of cluster state
- Minimal friction for application teams


Note:
*This platform is cloud-agnostic. I intentionally built it locally to focus on platform design, cost awareness, and reproducibility. The same patterns apply directly to EKS/GKE.*

*This is not a production platform or a SaaS product.*
*It is a hands-on lab designed to model real-world platform practices in a controlled environment.*


### Core Idea


The Application Team should be able to deploy service by commiting to Git -- without needing kubectl access, cluster credentials and deep Kubernetes knowledge

The Platform Team owns
- Namespace
- Deployment Standards
- Reconcilliation and Rollback

Developers Own
- Application Manifests
- Application Code

### High-Level Architecture

```bash
Git Repository
      ↓
ArgoCD (GitOps Controller)
      ↓
Kubernetes Cluster

```

