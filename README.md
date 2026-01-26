# Platform Engineering Lab

> **A GitOps-driven Kubernetes platform demonstrating enterprise platform engineering fundamentals**

This platform is intentionally built cloud-agnostic to focus on platform design patterns, cost awareness, and reproducibility. The same patterns apply directly to EKS, GKE, or any Kubernetes cluster.

## 🎯 What This Demonstrates

**Core Philosophy:** Developers deploy through Git commits only—never direct kubectl access. The platform enforces clear ownership boundaries between platform teams and application teams.

### Key Features
- ✅ **GitOps Single Source of Truth** - All infrastructure and applications defined in Git
- ✅ **Automated Reconciliation** - ArgoCD continuously syncs desired state
- ✅ **Namespace Governance** - Clear separation of concerns across teams
- ✅ **Developer Self-Service** - Git-based deployment without cluster credentials
- ✅ **Hierarchical Applications** - Scalable app-of-apps pattern
- ✅ **Auto-Healing & Pruning** - Automatic drift correction and cleanup
- ✅ **Rollback Capability** - Git history enables instant rollbacks

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Git Repository (main)                     │
│  ├── platform/ (infrastructure as code)                     │
│  │   ├── namespace/ (governance)                             │
│  │   └── cd/ (ArgoCD applications)                           │
│  └── apps/ (application manifests)                          │
│      ├── hello-app/ (sample nginx app)                      │
│      └── task-manager/ (custom application)                 │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  ArgoCD (GitOps Controller)                  │
│  ├── platform-root (manages platform infrastructure)        │
│  └── Individual apps (hello-app, task-manager)              │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│              Kubernetes Cluster (Local/EKS/GKE)              │
│  ├── argocd namespace (ArgoCD controller)                    │
│  ├── platform namespace (platform infrastructure)            │
│  ├── apps namespace (user applications)                      │
│  └── test namespace (testing workloads)                      │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- Kubernetes cluster (Kind, EKS, GKE, etc.)
- kubectl configured
- Git repository access

### 1. Install ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 2. Deploy Platform
```bash
# Create namespaces
kubectl apply -k platform/namespace

# Deploy platform root application
kubectl apply -f platform/cd/platform_root.yaml

# Wait for sync
kubectl get applications -n argocd
```

### 3. Access ArgoCD UI
```bash
# Get admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Port forward
kubectl port-forward svc/argocd-server -n argocd 8080:443

# Login at https://localhost:8080
# Username: admin
# Password: (from above command)
```

## 📊 Current Status

### ✅ Phase 1: Platform Bootstrap (COMPLETE)
- ArgoCD installed and configured
- GitOps wiring established  
- Namespace governance implemented
- Hierarchical application structure

### ✅ Phase 2: Application Onboarding (COMPLETE)
- Sample application (`hello-app`) deployed
- Custom application (`task-manager`) deployed
- Developer workflow documented
- GitOps-driven deployment proven

### 📋 Phase 3: Production Readiness (PLANNED)
- Multi-environment support (dev/staging/prod)
- Advanced RBAC policies
- Network policies and security
- Resource quotas and governance
- Monitoring and observability
- Secret management integration

## 🏢 Namespace Governance

| Namespace | Owner | Purpose | Applications |
|-----------|-------|---------|--------------|
| **argocd** | Platform Team | ArgoCD controller | ArgoCD components |
| **platform** | Platform Team | Platform infrastructure | Platform services |
| **apps** | Product Teams | User applications | hello-app, task-manager |
| **test** | DevOps Team | Testing workloads | Test applications |

## 📱 Current Applications

### hello-app (Reference Implementation)
- **Purpose:** Demonstrates GitOps deployment pattern
- **Image:** nginx:1.21
- **Replicas:** 4 with resource limits
- **Service:** ClusterIP on port 80
- **Status:** ✅ Running

### task-manager (Custom Application)  
- **Purpose:** Real application example with CI/CD
- **Image:** sumithsingh001/task-manager (auto-updated via GitHub Actions)
- **Replicas:** 1
- **Service:** ClusterIP 
- **Status:** ✅ Running

## 🔄 Deployment Workflow

### For Developers
1. **Create app directory:** `mkdir apps/your-app-name`
2. **Add manifests:** deployment.yaml, service.yaml, kustomization.yaml
3. **Deploy via Git:** `git add . && git commit -m "Add new app" && git push`
4. **Monitor:** ArgoCD automatically detects and deploys

### For Platform Team
1. **Platform config:** Modify files in `platform/`
2. **Namespace governance:** Update `platform/namespace/`
3. **ArgoCD apps:** Add new applications in `platform/cd/`

## 🎛️ Ownership Model

### Platform Team Owns
- Namespace creation and policies
- ArgoCD configuration
- Cluster infrastructure  
- Deployment standards
- Reconciliation and rollback

### Application Teams Own
- Application code and images
- Kubernetes manifests in `apps/your-app/`
- Application configuration
- Service dependencies

## 🔧 Rollback & Recovery

### Git-Based Rollback
```bash
# Revert commit
git revert <commit-hash>
git push origin main
# ArgoCD automatically syncs previous state
```

### ArgoCD UI Rollback
1. Open application in ArgoCD UI
2. Go to "History and Rollback" tab
3. Select previous revision
4. Click "Rollback"

## 📚 Documentation

- **[Setup Guide](Docs/README.md)** - Detailed setup and architecture
- **[Developer Guide](apps/README.md)** - Application deployment workflow
- **[Platform Guide](platform/)** - Platform configuration details

## 🌟 Real-World Applicability

This lab demonstrates patterns used in production environments:

- **Multi-team Kubernetes platforms** at scale
- **EKS/GKE enterprise deployments** 
- **Developer self-service platforms**
- **GitOps-first organizations**
- **Platform engineering teams**

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Follow the existing patterns
4. Test on local Kind cluster
5. Submit pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Key Principle:** Developers deploy through Git, never kubectl directly.