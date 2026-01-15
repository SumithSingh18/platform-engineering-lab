# Platform Engineering Lab

This platform is cloud-agnostic. I intentionally built it locally to focus on platform design, cost awareness, and reproducibility. The same patterns apply directly to EKS/GKE.


## Current Status

✅ **Phase 1 Complete:** Platform Bootstrap

- ArgoCD installed and configured
- GitOps wiring established
- Namespace governance implemented

🚧 **Phase 2 In Progress:** Application Onboarding

- Sample application (`hello-app`) deployed
- Developer workflow documented
- GitOps-driven deployment proven

## Quick Start

### For Developers

1. See `apps/README.md` for deployment guide
2. Follow the `hello-app` example
3. Deploy via Git commits only

### For Platform Team

1. Platform configuration in `platform/`
2. ArgoCD manages all deployments
3. Namespace policies enforced

## Architecture

```
Developer Git Commit → ArgoCD → Kubernetes Cluster
                    ↓
               apps/ directory
```

**Key Principle:** Developers deploy through Git, never kubectl directly.


#dummy